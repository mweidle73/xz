# Abuild XZ Utils archive

This repository preserves the historical XZ Utils source revision formerly
used by Abuild. The authoritative project is maintained by the
[Tukaani Project](https://github.com/tukaani-project/xz); the original XZ
Utils 5.2.5 documentation remains available in the repository's
[upstream README](README).

The long-lived branches have deliberately separate roles:

- `master` follows the official upstream `master` branch;
- `abuild` is the exact source revision formerly pinned by Abuild; and
- `abuild-gh` adds only this maintenance README and files below `.github/`
  to `abuild`.

The `abuild` branch is based on the official `v5.2.5` tag at `2327a461` and
adds one build-environment compatibility change at `24c3acc2`. That change
reverts the requirement for gettext 0.19.6 and restores the older gettext
0.19 requirement used by Abuild's former development environment.

Abuild no longer builds the vendored XZ sources; it switched to distribution
compression tools in 2026. The historical branch is retained for provenance
and reproducibility, not as a recommended XZ version for new deployments.

## Continuous integration

Run the same Trixie check locally with Docker:

```sh
.github/ci/run .github/ci/check
```

The image regenerates the Autotools files from the patched `configure.ac`, so
the sole Abuild change is part of the build rather than bypassed through the
release's pre-generated `configure` script. The check then copies that
generated source into a private temporary work area, configures and builds the
project, and runs the complete upstream test suite in a non-root, read-only
container without network access or Linux capabilities.

The weekly upstream monitor checks whether `master` still matches the
authoritative Tukaani repository. It reports drift but never updates branches
automatically.
