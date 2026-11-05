# CFSG source reduction experiment

This repository provides a reproducible **candidate source distribution** derived from [AxiomMath/CFSG](https://github.com/AxiomMath/CFSG). It applies two source patches to the fixed upstream commit `69cb972f587a3ecc035e092253167ff2e303c962`.

The candidate contains 42,494,665 LF lines in tracked Lean source files, compared with 43,068,005 at the upstream commit: a reduction of 573,340 lines (about 1.33%). This is a source-tree measurement, including imports, declarations, proof text, comments and generated data; it is not a claim of a shorter mathematical argument or faster compilation.

**Verification status:** selected changed modules and consumers have been compiled with Lean using cached dependencies, and the import modifications were checked with a source-derived loader model. The complete combined candidate has **not** been rebuilt from scratch, and the final classification theorem has **not** been checked on that full rebuilt candidate. The source tree hashes below establish reproducibility of the patches, not Lean correctness.

## Changes

1. `01-combined.patch.gz` shares an existing Feit–Thompson theorem implementation already used elsewhere in the classification, removes a redundant support tree, and includes local proof and certificate-construction reductions. This results in 339,157 fewer Lean LF lines. It deletes 1,409 old support modules: consumers importing those modules directly will need changes.
2. `02-imports.patch.gz` removes 234,183 additional candidate redundant import lines from the result of the first patch. This changes direct import metadata; source-level and sampled checks do not establish compatibility for every consumer or metaprogram.

The two patches are ordered and must both be applied. The separate 77-line exploratory patch is not included. Neither patch should be treated as an official upstream release.

## Reconstruct the candidate

Requires Git and gzip; cloning and building upstream require substantial storage and a compatible Lean toolchain. These commands modify the new clone's index and working tree; do not run in a checkout containing changes.

```sh
git clone https://github.com/AxiomMath/CFSG.git cfsg-candidate
cd cfsg-candidate
git checkout --detach 69cb972f587a3ecc035e092253167ff2e303c962

gzip -dc /path/to/cfsg-source-reduction/patches/01-combined.patch.gz | git apply --index --check -
gzip -dc /path/to/cfsg-source-reduction/patches/01-combined.patch.gz | git apply --index -
git write-tree
# Expected: 6d4b2747b81b2549c0810a47bb6be7cd193c4140

gzip -dc /path/to/cfsg-source-reduction/patches/02-imports.patch.gz | git apply --index --check -
gzip -dc /path/to/cfsg-source-reduction/patches/02-imports.patch.gz | git apply --index -
git write-tree
# Expected: 357b8236f1c4079bed3570234a1b3901feeb7943
```

Replace `/path/to/cfsg-source-reduction` with the absolute path to this repository. A complete from-scratch build and verification of the final theorem remain future work. Consult the pinned upstream README for its toolchain and build instructions; cached builds alone do not establish the full result.

## Attribution and licensing

The source excerpts in the patches are derived from AxiomMath/CFSG and its included third-party projects. The upstream root Apache 2.0 license is included as `LICENSE`. Copies of the upstream third-party license files at the pinned commit are in `upstream-licenses/`; upstream copyright and attribution notices remain in the patch text. Patches explicitly record changed lines and deleted files. Existing upstream and third-party licensing continues to apply to their respective material.
