---
name: installing-mlatom
description: Install MLatom and the AIQM3 add-ons in a clean environment, and diagnose the common installation failures.
mlatom: ">=3.23"
tags: [installation, aiqm3, environment]
---

# Installing MLatom (and the AIQM3 add-ons)

## When to use
A user wants to install MLatom locally, or install the **AIQM3** methods (the `aitomic-addons` package), or is hitting an install error such as *"No matching distribution found for aitomic-addons"*.

No installation needed for a quick try: MLatom and many of these methods run online on the [Aitomistic Hub](https://aitomistic.xyz).

## Prerequisites
- **Operating system:** MLatom's compiled backends and the AIQM3 add-ons run on **Linux** (x86-64 or ARM64/aarch64). There are **no macOS or Windows wheels** for the add-ons — on those systems use the online Hub, a Linux container, or WSL2.
- **Python:** 3.9, 3.10, or **3.11 (recommended)**.
  - Python ≤ 3.8 is **not** supported — the add-ons publish no wheels below 3.9.
  - Python ≥ 3.12 has no wheel for the `torch==2.1.2` that AIQM3 pins — stay on 3.9–3.11.
- **A clean environment** — install into a fresh conda/venv, not an old base environment, to avoid version conflicts.

## Steps

Create a fresh Python 3.11 environment and install MLatom + AIQM3:

```bash
conda create -n aiqm3 python=3.11 -y
conda activate aiqm3
pip install -U "numpy<2" torch==2.1.2 torchani==2.2.3 mlatom aitomic-addons joblib pyscf geometric
```

AIQM3 also uses D3 dispersion. MLatom runs `s-dftd3` as a separate program, so it must be on the system and `dftd3bin` must point at the executable:

```bash
conda install -c conda-forge simple-dftd3
export dftd3bin=$CONDA_PREFIX/bin/s-dftd3
```

The conda-forge package is called `simple-dftd3`. `pip install dftd3` installs Python bindings only — no `s-dftd3` executable — so it does not make AIQM3 work. Without it, AIQM3 fails at model construction, historically with a misleading "method is not detected in any of the interfaces" error.

For plain MLatom without the add-ons, `pip install -U mlatom` plus the dependencies you need is enough — see the [installation guide](http://mlatom.com/docs/installation.html).

## Verify

```python
import mlatom as ml

mol = ml.data.molecule.from_xyz_string('''3

O    0.00000    0.00000    0.11779
H    0.00000    0.75545   -0.47116
H    0.00000   -0.75545   -0.47116
''')
aiqm3 = ml.methods(method='AIQM3')
opt = ml.optimize_geometry(model=aiqm3, initial_molecule=mol).optimized_molecule
print(opt.energy)   # ~ -76.3787 Hartree
```

The neural-network parameters download automatically on first use, together with a one-time license notice.

## Troubleshooting

**`ERROR: No matching distribution found for aitomic-addons` (`from versions: none`)**
pip reached PyPI but found no wheel matching your Python version, OS, or CPU. Usual causes:
- Python is 3.7 or 3.8 → use 3.9–3.11. Check with `python --version`.
- The platform is macOS/Windows or a non-x86-64/aarch64 CPU → only Linux x86-64/aarch64 wheels are published, with no source distribution to fall back on.

Fix: create a fresh Python 3.11 environment on a supported Linux platform.

The same message for `torch==2.1.2` usually means your Python is **3.12 or newer** — use 3.9–3.11.

**`Temporary failure in name resolution` / repeated `Retrying`**
This is a **network** problem, not a compatibility one: the machine cannot reach PyPI (broken DNS, a proxy, or an offline node). The tell-tale sign is `Retrying` / name-resolution warnings printed *above* the `No matching distribution` line. Fix DNS/proxy/network, or install from a local PyPI mirror — the packages themselves are fine.

**`UnicodeDecodeError` from xtb**
Run under a UTF-8 locale: `export PYTHONUTF8=1`.

## Cite
- **MLatom** — see *Cite this repository* on the [MLatom GitHub page](https://github.com/dralgroup/mlatom) (the citation is also printed in the MLatom output and listed at [MLatom.com](http://mlatom.com)).
- **AIQM3** — Yuxinxin Chen, Yi-Fan Hou, Roman Zubatyuk, Olexandr Isayev, Pavlo O. Dral. *AIQM3: Targeting Coupled-Cluster Accuracy with Semi-Empirical Speed across Seven Main Group Elements.* J. Chem. Theory Comput. 2026. https://doi.org/10.1021/acs.jctc.5c01794
