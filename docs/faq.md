---
icon: material/help-circle-outline
---

# FAQ & Troubleshooting

## My Job Is Pending (`PD`). Why?

Check the reason in the last column of `squeue -u $USER`:

| Reason | Meaning | What to do |
|---|---|---|
| `Resources` | The requested GPUs/CPUs/memory aren't free yet | Wait, or request less / another partition |
| `Priority` | Other jobs are ahead in the queue | Wait |
| `QOSMaxGRESPerAccount`, `QOSMaxJobsPerAccount` (or similar `QOS...` reasons) | Your group reached its GPU or job limit | Wait for your group's jobs to finish — see [Cluster Resources](cluster-resources.md) |
| `ReqNodeNotAvail` | The node you asked for is down or reserved | Remove `--nodelist` or choose another node |

## `module: command not found`

The line `cluster_env module` in your `~/.bashrc` is missing or commented out — see [Shell Settings](environment/shell-settings.md). In batch scripts, add `source ~/.bashrc` before `module load`.

## `conda: command not found`

Check that `cluster_env conda` is on in `~/.bashrc` (or that your own conda setup is there), then open a new terminal. In batch scripts, add `source ~/.bashrc` before `conda activate`.

## `jupyter-start: command not found`

Check that `cluster_env jupyter` is on in `~/.bashrc`. Run `jupyter-start` only inside a job (after `salloc`).

## The Jupyter Link Doesn't Open

- Make sure the job is still running (`squeue -u $USER`).
- Copy the whole link, including `?token=...`.
- You must be on the university network (or VPN).

## My Job Was Killed

| Message in the output/error file | Cause | Fix |
|---|---|---|
| `DUE TO TIME LIMIT` | The job reached its `--time` | Request more time, or save checkpoints |
| `oom-kill` / `Out Of Memory` | Not enough RAM requested | Increase `--mem` |
| `CUDA out of memory` | Not enough GPU memory | Smaller batch size or model, or more/bigger GPUs |

To see how much memory a finished job used:

```bash
sacct -j <jobid> -o JobID,State,Elapsed,MaxRSS,ExitCode
```

## My Program Doesn't See the GPU

- Request a GPU: `--gres=gpu:1`. Without it, the job has no GPU.
- Inside the job, check with `nvidia-smi`.
- In containers, add `--nv` (see [Apptainer](software/apptainer.md)).

## I Can't SSH Into a Compute Node

That's expected — direct SSH to compute nodes isn't allowed. Use `salloc` for an interactive shell (see [Interactive Jobs](using-slurm/interactive-jobs.md)).

## Ollama Says the Address Is Already in Use

Another job on the same node is using that port. Set `OLLAMA_HOST` to a job-specific port before starting the server — see [Ollama](software/ollama.md).

## I'm Out of Disk Space

See [Storage](storage.md) for checking usage and clearing caches.

## Still Stuck?

Contact the JRCAI admin with your job ID, the command you ran, and the error message.
