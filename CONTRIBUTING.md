# Contributing to ModelFuzz

Thanks for your interest in contributing. This document covers how to set up a development environment, run tests, and submit changes.

## What This Project Accepts

ModelFuzz is a security library. People run it because they trust it to block calls that shouldn't execute, so every change is reviewed against a higher bar than "the tests pass."

**Discuss before you build.** Open an issue, or comment on an existing one, and wait for a reply before writing code. Unsolicited pull requests that arrive without prior discussion will usually be closed — not because the code is bad, but because reviewing an unrequested change costs more than the change is worth.

**Automated and AI-generated pull requests are not accepted.** Using an assistant while you work is fine and expected. What isn't accepted is a generated patch opened against an issue you haven't engaged with, from an account whose activity is drive-by PRs across unrelated repositories. A fork-to-PR gap measured in seconds is treated as a signal, and such PRs are closed without review.

**Claiming an issue means working on it.** Commenting "I'd like to work on this" on many repositories in quick succession isn't a contribution. If you claim an issue and nothing appears within a couple of weeks, it goes back in the pool — just say so if you get stuck or change your mind, which is completely fine.

**Changes to the guardrail core get extra scrutiny.** Anything touching [`rules.py`](src/modelfuzz/rules.py), [`engine.py`](src/modelfuzz/engine.py) or [`decorator.py`](src/modelfuzz/decorator.py) must state clearly what it changes about *blocking behavior*, and include a test proving that bad input is still blocked. A change that makes the library more permissive needs an explicit rationale.

None of this is meant to discourage genuine contributors. Say hello on an issue first and you'll get a real review.

## Development Setup

ModelFuzz uses [uv](https://github.com/astral-sh/uv) for dependency management.

```bash
git clone https://github.com/higagan/modelfuzz.git
cd modelfuzz
uv sync
```

This installs the package and its dependencies, including dev tools (`pytest`, `ruff`), into a local virtual environment.

## Running Tests

```bash
uv run pytest
```

Tests live under [tests/](tests/) and are organized by module (`test_decorator.py`, `test_engine.py`, `test_rules.py`). Add tests alongside the code they cover, and make sure new policies or behaviors are exercised by at least one test.

## Linting

```bash
uv run ruff check .
```

CI runs lint and tests against Python 3.10, 3.11, and 3.12 on every push and pull request (see [.github/workflows/ci.yml](.github/workflows/ci.yml)). Please make sure both pass locally before opening a PR.

## Submitting Changes

1. **Open or comment on an issue first, and wait for a reply.** See [What This Project Accepts](#what-this-project-accepts).
2. Fork the repository and create a branch from `main`.
3. Make your changes, with tests covering new behavior.
4. Run `uv run ruff check .` and `uv run pytest` locally.
5. Open a pull request that links the issue and describes the change and its motivation.

Note that CI does not run automatically on pull requests from first-time contributors — it needs a maintainer to approve the workflow run. If your checks look like they haven't started, that's why, and it isn't something you need to fix.

## Reporting Issues

Use [GitHub Issues](https://github.com/higagan/modelfuzz/issues) to report bugs or propose features. Include a minimal reproduction where possible.

**Security vulnerabilities do not belong in the issue tracker.** See [SECURITY.md](SECURITY.md) for how to report them privately.

Before reporting a bypass of the default policies, please read the [Limitations](README.md#limitations) section. The default `SensitiveDataFilter` matches keywords only and is documented as not being a credential detector, so payloads it doesn't catch are known behavior rather than vulnerabilities.

## License

By contributing, you agree that your contributions will be licensed under the project's [MIT License](LICENSE).
