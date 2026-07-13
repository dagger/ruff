# ruff

A [Dagger](https://dagger.io) module — written in the `.dang` module language —
that wraps [ruff](https://docs.astral.sh/ruff/), the extremely fast Python
linter and formatter.

Ruff ships as a self-contained static binary, so this module extracts it from
the pinned `ghcr.io/astral-sh/ruff` image (tracked by Dependabot via
[`images/ruff/Dockerfile`](images/ruff/Dockerfile)) and runs it on an Alpine
base — no Python, `pip`, `uv`, or virtualenv required.

## Functions

| Function       | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| `check`        | Lint with `ruff check`; fails on violations (a `@check`).               |
| `format-check` | Verify formatting with `ruff format --check`; fails if unformatted.     |
| `lint`         | Return the lint report as text without failing on violations.           |
| `fix`          | Apply `ruff check --fix` and return the modified source directory.      |
| `format`       | Run `ruff format` and return the formatted source directory.            |
| `version`      | The version of the bundled ruff binary.                                  |

The constructor takes the project `source` from the workspace at `sourcePath`
(default `/`). `args` are forwarded to every ruff invocation, and `container`
overrides the default Alpine + ruff base.

## Usage

Install the module in your workspace:

```sh
dagger install github.com/dagger/ruff
```

Check for lint and format issues:

```sh
dagger check
```

Or to only run the ruff module:

```sh
dagger check ruff
```

To apply the formatter:

```sh
dagger generate
dagger generate ruff
```
