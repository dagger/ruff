# CWD-aware Ruff discovery

`projects(ws)` delegates discovery of `pyproject.toml`, `ruff.toml`, and
`.ruff.toml` to `github.com/dagger/polyfill`. Results are cwd-relative: the cwd
and descendants plus at most one nearest enclosing project.

`checkAll` checks only the caller's project and descendants, never a strict
ancestor represented by a `..` path. Explicit `check`, `lint`, `fix`, and
`format` calls continue to use `sourcePath`.

```console
dagger check -l
dagger call ruff discovery-check
dagger call ruff cwd-scope-check
dagger check ruff:check-all
```

`tests/discovery/ancestor` is a runnable repro tree. `cwd-scope-check` runs from
its configless `work` directory, injects a syntax error into the ancestor and
proves it is skipped, then injects one into `work/app` and proves it is checked.
