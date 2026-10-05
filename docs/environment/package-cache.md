---
icon: material/package-variant-closed
---

# Package Cache (Nexus)

The cluster has a local package cache. `pip` and `uv` download through it, so packages that someone already installed come from inside the network instead of the internet — faster, and it saves bandwidth.

It is on by default through the line `cluster_env nexus` in your `~/.bashrc` (see [Shell Settings](shell-settings.md)). You don't need to change how you install packages.

```bash
pip install torch torchvision torchaudio
```

## Help

```bash
nexus-help
```

Shows installation examples for common AI libraries.

## Skip the Cache for One Install

If a package version is missing from the cache, install that one package directly from PyPI:

```bash
pip install <package> --index-url https://pypi.org/simple
```

For conda, using the default channels only:

```bash
conda install <package> -c defaults --override-channels
```

## Turn the Cache Off

1. In `~/.bashrc`, comment out the line:

    ```bash
    # cluster_env nexus       # JRCAI package cache
    ```

2. If your `~/.condarc` lists JRCAI cache channels, comment those out too.
3. Open a new terminal.

!!! note

    In batch jobs the cache settings apply if your script runs `source ~/.bashrc` (recommended).
