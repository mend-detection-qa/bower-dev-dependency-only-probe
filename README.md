# bower-dev-dependency-only-probe

This probe validates Mend's group classification for Bower projects that declare both `dependencies` and `devDependencies`. `jquery` is the sole production dependency and must be reported in the `main` group. `jasmine-core` and `mocha` are declared under `devDependencies` and must be reported in the `dev` group. Bower does not install transitive devDependencies of installed packages, so the resolved tree should contain at most 3 packages total (1 main + 2 dev root-level). Any transitive devDependency packages appearing in Mend scan output indicates a Mend classification or over-resolution bug. The project is manifest-only: `bower_components/` is not committed and Mend must derive the dependency graph solely from `bower.json`.

## Feature exercised

Group classification of `dependencies` vs `devDependencies` in a Bower manifest, and verification that Bower's rule of not installing transitive devDependencies is reflected in Mend's output.

## Expected dependency tree

- `jquery` — registry, group `main`, direct, parent: root
- `jasmine-core` — registry, group `dev`, direct, parent: root
- `mocha` — registry, group `dev`, direct, parent: root
- Total: 3 packages (1 main, 2 dev, 0 transitive)
- No `resolutions` field used
- No lockfile present (manifest-only detection)
- `bower_components/` not committed

## Probe metadata

| Field | Value |
|---|---|
| pattern | dev-dependency-only |
| pm | bower |
| detection_mode | manifest-only |
| target | remote |
| repo | mend-detection-qa/bower-dev-dependency-only-probe |
| generated | 2026-05-04 |