# Fork Open WebUI 👋

> [!IMPORTANT]
> This repository is a **fork of [Open WebUI](https://github.com/open-webui/open-webui)**. It keeps the upstream project as its foundation while providing a dedicated place for fork-specific changes, experiments, deployment defaults, and documentation.

![GitHub stars](https://img.shields.io/github/stars/open-webui/open-webui?style=social)
![GitHub forks](https://img.shields.io/github/forks/open-webui/open-webui?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/open-webui/open-webui?style=social)
![GitHub repo size](https://img.shields.io/github/repo-size/open-webui/open-webui)
![GitHub language count](https://img.shields.io/github/languages/count/open-webui/open-webui)
![GitHub top language](https://img.shields.io/github/languages/top/open-webui/open-webui)
![GitHub last commit](https://img.shields.io/github/last-commit/open-webui/open-webui?color=red)
[![Discord](https://img.shields.io/badge/Discord-Open_WebUI-blue?logo=discord&logoColor=white)](https://discord.gg/5rJgQTnV4s)
[![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/open-webui)

![Open WebUI Banner](./banner.png)

## Sections

- [Fork status](#fork-status)
- [What this fork is for](#what-this-fork-is-for)
- [Upstream Open WebUI](#upstream-open-webui)
- [Highlights](#highlights)
- [Installation](#installation)
- [Docker quick start](#docker-quick-start)
- [Python quick start](#python-quick-start)
- [Configuration notes](#configuration-notes)
- [Development](#development)
- [Keeping this fork updated](#keeping-this-fork-updated)
- [Troubleshooting](#troubleshooting)
- [Security](#security)
- [License](#license)
- [Support](#support)

## Fork status

This is not the canonical Open WebUI repository. The upstream project lives at [`open-webui/open-webui`](https://github.com/open-webui/open-webui), and this fork may contain changes that are not present upstream.

Use this repository when you want the behavior, configuration, or documentation maintained by this fork. Use upstream Open WebUI when you want the official project source, releases, issue tracker, and community support channels.

## What this fork is for

This fork exists to make local customization easier while preserving Open WebUI's core experience. Typical fork-specific work may include:

- deployment defaults for a specific environment;
- README and operator documentation tailored to this fork;
- experimental changes before they are proposed upstream;
- integration adjustments that are too specific for upstream defaults;
- patches that need to be carried while waiting for upstream review.

When possible, broadly useful improvements should still be proposed to upstream Open WebUI.

## Upstream Open WebUI

**Open WebUI** is an extensible, feature-rich, self-hosted AI platform designed to run with local and remote model providers. It supports Ollama, OpenAI-compatible APIs, built-in RAG workflows, tools, plugins, multimodal workflows, authentication options, and production deployment patterns.

Helpful upstream resources:

- Documentation: <https://docs.openwebui.com/>
- Upstream repository: <https://github.com/open-webui/open-webui>
- Community site: <https://openwebui.com/>
- Discord: <https://discord.gg/5rJgQTnV4s>

![Open WebUI Demo](./demo.png)

## Highlights

- 🚀 **Easy deployment** with Docker, Docker Compose, Kubernetes, Helm, `pip`, and `uv` workflows.
- 🤝 **Broad model support** for Ollama and OpenAI-compatible providers such as LM Studio, Groq, Mistral, OpenRouter, vLLM, and more.
- 🔐 **Administration controls** including users, groups, roles, permissions, authentication, and enterprise identity integrations.
- 🧩 **Extensibility** through tools, filters, actions, pipes, functions, plugins, OpenAPI tool servers, MCP, and MCPO.
- 📚 **RAG and knowledge workflows** with multiple vector databases, document extractors, web search providers, reranking, and hybrid search.
- 🎤 **Voice, video, and multimodal features** with speech-to-text, text-to-speech, image generation, image editing, and file workflows.
- 📊 **Operational features** including observability, usage analytics, model evaluation, scalable storage, PostgreSQL, Redis, and OpenTelemetry.
- 📱 **Responsive PWA experience** for desktop and mobile use.

## Installation

Choose the path that matches how you want to run this fork. The commands below use upstream Open WebUI container and package names unless this fork publishes its own artifacts.

Before installing, decide whether you want:

- **Docker** for the fastest repeatable deployment;
- **Python** for a lightweight local installation;
- **Docker Compose, Kubernetes, Kustomize, or Helm** for production-style environments.

For full upstream installation details, see the [Open WebUI getting-started documentation](https://docs.openwebui.com/getting-started/).

## Docker quick start

> [!WARNING]
> Always mount `/app/backend/data` to persistent storage. Without a volume, upgrades or container removal can delete application data.

### Open WebUI with Ollama on the same host

```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

Then open <http://localhost:3000>.

### Open WebUI with Ollama on another server

```bash
docker run -d \
  -p 3000:8080 \
  -e OLLAMA_BASE_URL=https://example.com \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

### Open WebUI for OpenAI-compatible API usage

```bash
docker run -d \
  -p 3000:8080 \
  -e OPENAI_API_KEY=your_secret_key \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

### Open WebUI with NVIDIA GPU support

```bash
docker run -d \
  -p 3000:8080 \
  --gpus all \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:cuda
```

### Open WebUI bundled with Ollama

```bash
docker run -d \
  -p 3000:8080 \
  --gpus=all \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama
```

For CPU-only bundled Ollama usage, remove `--gpus=all`.

## Python quick start

Open WebUI can be installed with `pip`. Python 3.11 is recommended for compatibility.

```bash
pip install open-webui
open-webui serve
```

Then open <http://localhost:8080>.

## Configuration notes

- Use `HF_HUB_OFFLINE=1` in offline environments to prevent Hugging Face Hub download attempts.
- Use `OLLAMA_BASE_URL` when Ollama runs outside the Open WebUI container.
- Use `OPENAI_API_KEY` and provider-specific base URL settings for OpenAI-compatible providers.
- Use persistent volumes or external storage for any deployment that must survive restarts, upgrades, or container replacement.

```bash
export HF_HUB_OFFLINE=1
```

## Development

This fork follows the upstream Open WebUI project structure. Common developer entry points include:

- `backend/` for the backend service;
- `src/` for the web application frontend;
- `docs/` for documentation and security policy files;
- `docker-compose*.yaml` files for containerized development and deployment variants.

Check the project scripts and upstream documentation before changing build, test, or deployment behavior.

## Keeping this fork updated

Because this repository is a fork, periodically compare it with upstream Open WebUI and decide which upstream changes to merge, rebase, or cherry-pick.

A typical workflow is:

```bash
git remote add upstream https://github.com/open-webui/open-webui.git
git fetch upstream
git checkout main
git merge upstream/main
```

Resolve conflicts carefully so fork-specific changes remain intentional and visible.

## Troubleshooting

If the Open WebUI container cannot reach Ollama on the host, try host networking on Linux:

```bash
docker run -d \
  --network=host \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://127.0.0.1:11434 \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

With host networking, Open WebUI is available at <http://localhost:8080> instead of <http://localhost:3000>.

For more help, see the upstream [troubleshooting documentation](https://docs.openwebui.com/troubleshooting/) and this repository's [`TROUBLESHOOTING.md`](./TROUBLESHOOTING.md).

## Security

Security issues should be handled carefully and not disclosed publicly before maintainers can investigate. For upstream Open WebUI security reporting, use the [GitHub security advisory process](https://github.com/open-webui/open-webui/security). Also review this repository's security policy in [`docs/SECURITY.md`](./docs/SECURITY.md).

## License

This project contains code under multiple licenses. The current codebase includes components licensed under the Open WebUI License with an additional requirement to preserve the "Open WebUI" branding, as well as prior contributions under their respective original licenses.

Review [`LICENSE`](./LICENSE), [`LICENSE_HISTORY`](./LICENSE_HISTORY), and [`LICENSE_NOTICE`](./LICENSE_NOTICE) for complete licensing details.

## Support

For fork-specific issues, use this fork's issue tracker or pull requests. For general Open WebUI usage, upstream documentation and community channels are usually the best starting point.

---

Open WebUI was created by [Timothy Jaeryang Baek](https://github.com/tjbck). This fork builds on that work while making its fork status explicit.
