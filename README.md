# Canton CI/CD Templates

Ready-to-use GitHub Actions workflows for [Canton Network](https://www.canton.network/) / [Daml](https://www.daml.com/) projects.

Drop these into any Daml project to get automated builds, tests, and release packaging — no configuration needed beyond a standard `daml.yaml`.

## What's Included

| Workflow | Trigger | What it does |
|---|---|---|
| `daml-ci.yml` | Push / PR to main | Installs Daml SDK → builds → runs tests → uploads DAR artifact |
| `daml-release.yml` | Push version tag (`v*`) | Builds → tests → creates GitHub Release with DAR attached |

## Quick Start

**1. Copy the workflow files into your project:**

```bash
# From your Daml project root
mkdir -p .github/workflows

# Copy the CI workflow
curl -o .github/workflows/daml-ci.yml \
  https://raw.githubusercontent.com/YOUR_ORG/canton-ci-templates/main/.github/workflows/daml-ci.yml

# Copy the release workflow
curl -o .github/workflows/daml-release.yml \
  https://raw.githubusercontent.com/YOUR_ORG/canton-ci-templates/main/.github/workflows/daml-release.yml
```

**2. Ensure your project has a `daml.yaml` with an `sdk-version`:**

```yaml
sdk-version: 2.10.0
name: my-canton-app
version: 0.1.0
source: daml
dependencies:
  - daml-prim
  - daml-stdlib
  - daml-script
```

**3. Push to GitHub.** The CI pipeline runs automatically.

**4. To create a release:**

```bash
git tag v0.1.0
git push origin v0.1.0
```

This triggers the release workflow, which builds your DAR and attaches it to a GitHub Release.

## How It Works

The workflows handle the full Daml build lifecycle:

1. **Java 21** — Installed via `actions/setup-java` (Daml SDK requires Eclipse Temurin JDK 21)
2. **Daml SDK** — Version auto-detected from your `daml.yaml`. Installed via the official installer and cached between runs.
3. **`daml build`** — Compiles your Daml source into a `.dar` (Daml Archive) file
4. **`daml test`** — Runs all Daml Script tests in your project
5. **Artifact upload** — The compiled DAR is uploaded as a build artifact (CI) or attached to a GitHub Release (release workflow)

## Configuration

### Project in a subdirectory

If your `daml.yaml` isn't at the repo root, update the `DAML_PROJECT_DIR` environment variable:

```yaml
env:
  DAML_PROJECT_DIR: "packages/my-contracts"
```

### Different SDK version

Just update `sdk-version` in your `daml.yaml`. The workflow reads it automatically — no workflow changes needed.

### Adding a format check

The CI workflow includes a commented-out lint/format job. Uncomment it to enforce Daml formatting standards on PRs.

## Sample Project

The `sample-project/` directory contains a minimal Daml project (IOU contract with tests) that demonstrates the CI templates in action. The `sample-ci.yml` workflow runs against it on every push.

## Requirements

- A GitHub repository
- A Daml project with a valid `daml.yaml`
- Daml Script tests (recommended but the build step works without them)

No Docker, no special runners, no secrets. Everything runs on standard GitHub-hosted Ubuntu runners.

## Compatibility

- **Daml SDK**: Tested with 2.x. Should work with 3.x (update `sdk-version` in your `daml.yaml`).
- **GitHub Actions**: Uses `ubuntu-latest` runners
- **Java**: Eclipse Temurin JDK 21

## Contributing

Found an issue or want to add a workflow variant? PRs welcome.

## License

[BSD Zero Clause License](LICENSE) — Use however you want.

## Links

- [Canton Network](https://www.canton.network/)
- [Daml Documentation](https://docs.daml.com/)
- [Canton Network Quickstart](https://github.com/digital-asset/cn-quickstart)
- [Daml SDK Releases](https://github.com/digital-asset/daml/releases)
- [Canton Developer Resources](https://www.canton.network/developer-resources)
