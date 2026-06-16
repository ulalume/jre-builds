# jre-builds

Minimal Java runtimes (built with `jlink`) per platform, so SGDK's Java tools
(`rescomp`, `sizebnd`, `xgm2tool`, ...) run without a system-wide JDK. `sgdkx`
downloads `jre-<platform>.tar.gz` and puts `<jre>/bin` on the build PATH.

Targets: linux x86_64 / arm64, macOS arm64 / x86_64.
(Windows uses a system Java, as SGDK does natively.)

## How it works

`build.yml` installs **Eclipse Temurin** (OpenJDK) via `actions/setup-java`, then
runs `jlink --add-modules java.se` to produce a stripped, compressed runtime, and
packages it as `jre-<platform>.tar.gz` (top-level `jre/`).

- Trigger: manual / tag `jdk*` (e.g. `jdk21-1`).
- `java.se` aggregates the full SE API — safe for rescomp without per-jar module
  analysis. Trim later with `jdeps` if a smaller runtime is wanted.

## Licenses

- **This repository** (workflow): GPL-3.0-or-later (see `LICENSE`).
- The produced runtime is **OpenJDK (Eclipse Temurin)** under **GPL-2.0 with the
  Classpath Exception** — free to redistribute. The Classpath Exception means
  bundling/running other programs on it does not make them GPL. `jlink` preserves
  the runtime's `legal/` notices inside each bundle.
