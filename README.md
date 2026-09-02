# Oh CI!

> Minimal tooling for CI via O(h)CI.

Oh CI is an Alpine based image maintained for bash/sh style CI in a container environment. Just the essentials, like trees, and Pokémon because it is not safe out there in this world if you do not have at least one.

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/PrPlanIT/oh-CI) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/PrPlanIT/oh-CI) [![license](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/license.svg)](https://github.com/PrPlanIT/oh-CI/blob/main/LICENSE) [![Open Issues](https://img.shields.io/github/issues/PrPlanIT/oh-CI)](https://github.com/PrPlanIT/oh-CI/issues) [![Open PRs](https://img.shields.io/github/issues-pr/PrPlanIT/oh-CI)](https://github.com/PrPlanIT/oh-CI/pulls) [![Contributors](https://img.shields.io/github/contributors/PrPlanIT/oh-CI)](https://github.com/PrPlanIT/oh-CI/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/PrPlanIT)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![release](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/release.svg)](https://github.com/PrPlanIT/oh-CI/releases) [![build](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/build.svg)](https://gitlab.prplanit.com/PrPlanIT/oh-CI/-/pipelines) [![Last Commit](https://img.shields.io/github/last-commit/PrPlanIT/oh-CI)](https://github.com/PrPlanIT/oh-CI/commits) [![StageFreight](https://img.shields.io/badge/StageFreight-0.10.0--dev+b340e94-310937?logo=readthedocs&logoColor=white)](https://stagefreight.prplanit.com)
<!-- sf:badges:end -->
<!-- sf:image:start -->
[![GHCR](https://img.shields.io/badge/GHCR-prplanit%2Foh--ci-181717?logo=github&logoColor=white)](https://github.com/PrPlanIT/oh-CI/pkgs/container/oh-ci) [![Docker](https://img.shields.io/badge/Docker-prplanit%2Foh--ci-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/prplanit/oh-ci) [![pulls](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/pulls.svg)](https://hub.docker.com/r/prplanit/oh-ci) [![Harbor](https://img.shields.io/badge/Harbor-prplanit%2Foh--ci-60b932)](https://cr.pcfae.com/harbor/projects)

[![latest](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/release-latest.svg)](https://github.com/PrPlanIT/oh-CI/pkgs/container/oh-ci) ![updated](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/release-updated.svg) [![size](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/release-size.svg)](https://github.com/PrPlanIT/oh-CI/pkgs/container/oh-ci) [![latest-dev](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/dev-latest.svg)](https://github.com/PrPlanIT/oh-CI/pkgs/container/oh-ci) ![updated](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/dev-updated.svg) [![size](https://raw.githubusercontent.com/PrPlanIT/oh-CI/main/.stagefreight/scribe/dev-size.svg)](https://github.com/PrPlanIT/oh-CI/pkgs/container/oh-ci)
<!-- sf:image:end -->

---

## What's Inside

| Tool | Why It's Here |
|------|---------------|
| `bash` | Because `sh` alone is pain |
| `coreutils` | The GNU classics — `date`, `stat`, `env`, friends |
| `curl` | Talk to things |
| `gettext` | Provides `envsubst` — template all the things |
| `git` | Clone, fetch, diff |
| `jq` | Speak JSON fluently |
| `rsync` | Move files with grace |
| `tree` | See what you're working with |
| `yq` | Like `jq` but for YAML |
| `krabby` | Display Pokémon in your terminal. No, it's not negotiable. |

---

## Quick Start

```bash
# Run it
docker run --rm -it docker.io/prplanit/oh-ci:latest

# Template a config file from environment variables
docker run --rm \
  -e DB_HOST=postgres.local \
  -e DB_PORT=5432 \
  -v ./template.conf:/template.conf \
  docker.io/prplanit/oh-ci:latest \
  sh -c 'envsubst < /template.conf'

# Greet yourself properly
docker run --rm docker.io/prplanit/oh-ci:latest krabby random
```

---

## Use Cases

| Scenario | Example |
|----------|---------|
| **K8s init container** | Render config templates with `envsubst` before the app starts |
| **CI pipeline step** | Clone a repo, `jq` an API response, `rsync` artifacts |
| **Config generation** | Substitute `${VAR}` placeholders in YAML/HCL/TOML files |
| **Debugging** | Drop into a pod with actual tools instead of crying into busybox |

---

## Kubernetes Init Container Example

```yaml
initContainers:
  - name: render-config
    image: docker.io/prplanit/oh-ci:latest
    command: ["/bin/sh", "-c"]
    args:
      - envsubst < /template/app.conf > /config/app.conf
    envFrom:
      - secretRef:
          name: app-secrets
    volumeMounts:
      - name: template
        mountPath: /template
      - name: config
        mountPath: /config
```

---

## Looking For Something Else?

| Image | What It's For |
|-------|---------------|
| [`hlhd/ansible`](https://hub.docker.com/r/hlhd/ansible) | Ansible playbooks, Python, collections, the whole orchestra |
| [`prplanit/stagefreight`](https://hub.docker.com/r/prplanit/stagefreight) | Declarative CI/CD — detect, build, scan, and release container images |
| [`alpine/k8s`](https://hub.docker.com/r/alpine/k8s) | kubectl, helm, and the Kubernetes toolkit |

---

## Building

```bash
docker build -t prplanit/oh-ci:latest .
```

---

## License

Distributed under the [AGPL-3.0-only](LICENSE) License. See [LICENSING.md](docs/LICENSING.md) for commercial licensing.
