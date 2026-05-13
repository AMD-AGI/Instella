# Contributing to Instella

Thanks for your interest in contributing to **Instella**! This document explains how to file issues, propose changes, and get a pull request through review.

If you're reporting a security vulnerability, please **do not** open a public issue or PR — follow the process in [`SECURITY.md`](./SECURITY.md) instead.

## Code of Conduct

By participating in this project, you agree to behave respectfully and professionally toward other contributors. Harassment, personal attacks, and discriminatory language will not be tolerated.

## Ways to Contribute

- **Bug reports** — open a [GitHub issue](https://github.com/AMD-AGI/Instella/issues) with reproduction steps, the affected commit/release, your environment (Python version, PyTorch/ROCm version, GPU), and the actual vs. expected behavior.
- **Feature requests** — open an issue describing the use case, the proposed change, and any alternatives you considered. For larger changes, please discuss the design before sending a PR.
- **Documentation improvements** — typo fixes, clarifications, and new examples are welcome via PR.
- **Code contributions** — bug fixes, performance improvements, new training/evaluation scripts, etc. See the workflow below.

## Development Setup

Clone the repository and install in editable mode with the development extras:

```bash
git clone https://github.com/AMD-AGI/Instella.git
cd Instella
# Install Flash-Attention on MI300X (skip if not training on AMD GPU)
GPU_ARCH=gfx942 MAX_JOBS=$(nproc) pip install git+https://github.com/Dao-AILab/flash-attention.git -v
# Install all dependencies including dev tooling
pip install -e .[all]
```

The `[all]` extra installs both `dev` (linters, formatters, pytest) and `train` (wandb, datasets, etc.) dependency groups defined in [`pyproject.toml`](./pyproject.toml).

## Pull Request Workflow

1. **Fork** the repository and create a feature branch from `main`:
   ```bash
   git checkout -b your-feature-name
   ```
2. **Make your changes** in small, focused commits with clear messages.
3. **Run formatters and linters** locally before pushing (see below).
4. **Add or update tests** under `tests/` for any code changes that affect behavior.
5. **Run the test suite** locally:
   ```bash
   pytest tests/
   ```
6. **Push** your branch and open a pull request against `main`. Fill in the PR description with:
   - What the change does and why
   - How you tested it
   - Any related issues (`Fixes #123`)
7. **Address review feedback** by pushing additional commits to the same branch.

### Review Requirements

- Every PR requires review and approval from at least one code owner listed in [`.github/CODEOWNERS`](./.github/CODEOWNERS) before it can be merged.
- All required CI checks must pass.
- Direct pushes to `main` are not permitted.

## Code Style

This repo uses the following tools, all configured in [`pyproject.toml`](./pyproject.toml):

- **`black`** — code formatter, line length 115
- **`isort`** — import sorter, configured with the `black` profile
- **`ruff`** — linter (ignores `F403`, `F405`, `E501`)
- **`mypy`** — static type checker

Format and lint before pushing:

```bash
black .
isort .
ruff check .
mypy .
```

The `pretrain_data/` and `inference/` directories are excluded from most checks — see `pyproject.toml` for the full exclusion lists.

## Testing

- Place new tests under `tests/`, following the pytest discovery rules in `pyproject.toml` (`Test*` classes / `*Test` classes).
- Mark GPU-dependent tests with `@pytest.mark.gpu` so CPU-only environments can skip them:
  ```python
  import pytest

  @pytest.mark.gpu
  def test_something_on_gpu():
      ...
  ```
- Run the full suite with `pytest tests/`, or a subset with `pytest tests/path/to/test_file.py::TestClass::test_name`.

## Commit Messages

Use clear, imperative-mood commit messages:

```
Fix off-by-one in tokenizer batching

The previous implementation dropped the final token when the input
length was an exact multiple of the chunk size. Add a regression test.
```

Squash trivial fixup commits before requesting final review, when practical.

## Configuration & Secrets

- **Never** commit credentials, API keys, tokens, model weights under NDA, or customer data — see `SECURITY.md` for details.
- Local secrets belong in `.env` files (already excluded by [`.gitignore`](./.gitignore)).
- Configuration templates live in [`configs/`](./configs/); copy and modify rather than editing checked-in configs in place when running experiments.

## Reporting Security Issues

Security vulnerabilities should be reported privately through the process in [`SECURITY.md`](./SECURITY.md) — do **not** use the public issue tracker.

## License

By contributing to Instella, you agree that your contributions will be licensed under the same terms as the rest of the repository (see [`LICENSE`](./LICENSE) and [`NOTICES`](./NOTICES)).

## Questions

If you're unsure about anything — the right place to file an issue, whether a change is in scope, how to structure a PR — please open a draft issue or reach out to the code owners listed in [`.github/CODEOWNERS`](./.github/CODEOWNERS). We'd rather answer a question early than ask you to redo work later.
