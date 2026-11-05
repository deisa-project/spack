# deisa-project Spack packages

A [Spack](https://spack.io) package repository for
[deisa-project](https://github.com/deisa-project) software.

Currently provides:

| Spack package | Upstream project |
|---------------|------------------|
| `py-deisa-ray` | [deisa-project/deisa-ray](https://github.com/deisa-project/deisa-ray) — In Situ Analytics with Ray backend |

This repository exists so that the `py-deisa-ray` recipe can be updated
independently of the upstream [spack/spack-packages](https://github.com/spack/spack-packages)
repository. When this repo is registered *before* `builtin`, Spack uses the
recipe here instead, so you get new releases as soon as this repo is updated —
you don't have to wait for the upstream PR to be merged.

## Repository layout

```
.
├── spack-repo-index.yaml          # lets Spack locate the repo inside this git repo
└── spack_repo/
    └── deisa/                     # namespace: deisa
        ├── repo.yaml              # repo: namespace: deisa, api: v2.4
        └── packages/
            └── py_deisa_ray/
                └── package.py     # the py-deisa-ray recipe
```

## Requirements

- Spack **v1.0+** (this repo uses package API v2).

## Register the repository

**Option A — from a local clone (recommended):**

```bash
git clone https://github.com/deisa-project/spack.git
spack repo add "$PWD/spack/spack_repo/deisa"
```

**Option B — let Spack clone and manage it** (uses `spack-repo-index.yaml`):

```bash
spack repo add https://github.com/deisa-project/spack.git
```

**Option C — declare it in an environment (`spack.yaml`):**

```yaml
spack:
  repos:
    deisa: /path/to/spack/spack_repo/deisa
  specs:
    - py-deisa-ray
```

> This repository is private, so cloning over HTTPS requires a token, or use
> SSH: `git clone git@github.com:deisa-project/spack.git`.

Verify it is picked up:

```bash
spack repo list          # 'deisa' should be listed (and appear before 'builtin')
spack info py-deisa-ray
```

## Install

```bash
spack install py-deisa-ray                 # latest version
spack install py-deisa-ray@0.1.9           # a specific version
spack install py-deisa-ray@0.1.9 +example  # with the getting-started example deps (matplotlib)
```

The `+example` variant installs `py-matplotlib`, which is only needed to run the
getting-started example.

## Updating the recipe

1. Edit `spack_repo/deisa/packages/py_deisa_ray/package.py`.
2. Commit and push.
3. Consumers update and re-install:

```bash
# local clone:
git pull

# or, if Spack manages the clone:
spack repo update deisa

spack install py-deisa-ray
```

### Adding a new version

Always generate the `version(...)` line with Spack so the checksum is correct:

```bash
spack checksum py-deisa-ray <version>
```

Then paste the printed line into `package.py` (newest version first, and don't
remove older versions).

## Notes

- When registered, this repo **overrides** the `py-deisa-ray` recipe shipped in
  `spack-packages` (repo order matters: this one must come first).
- The recipe files derive from
  [spack/spack-packages](https://github.com/spack/spack-packages)
  (licensed Apache-2.0 OR MIT) and keep their original license headers.

## License

MIT — see [LICENSE](LICENSE).
