# SLURM Users Guide — KFUPM JRCAI

A guide to using SLURM on KFUPM JRCAI clusters, built with [Zensical](https://zensical.org/).

[![View Docs](https://img.shields.io/badge/docs-kfupm--jrcai.github.io-284475?style=for-the-badge&logo=readthedocs&logoColor=white)](https://kfupm-jrcai.github.io/slurm-users-guide/)

## Structure

- `docs/using-slurm/` — monitoring, submitting jobs, interactive jobs, transferring data, account commands.
- `docs/environment/` — shell settings, modules, package cache, and the model zoo.
- `docs/software/` — Ollama, Apptainer.
- `docs/cluster-resources.md`, `docs/group-policies.md`, `docs/how-to-connect.md`, `docs/storage.md`, `docs/job-script-templates.md`, `docs/jupyter-access.md`, `docs/faq.md` — flat top-level pages.

## How Changes Go Live

Everything syncs through GitHub — there's no manual deploy step:

1. Edit files under `docs/`, and the nav in `zensical.toml` if you added/moved/renamed a page.
2. Commit and push to `main`.
3. `.github/workflows/docs.yml` builds the site (`zensical build --clean`) and deploys it to **GitHub Pages**.
4. Live within a minute or two at [kfupm-jrcai.github.io/slurm-users-guide](https://kfupm-jrcai.github.io/slurm-users-guide/) — public, no sign-in (this guide has no sensitive infrastructure details, unlike the admin guide).

## Local Development

```bash
uv run zensical serve   # preview
uv run zensical build   # one-off build, output in site/
```

*By: Mohammed AlSinan (mohammed.sinan@kfupm.edu.sa)*
