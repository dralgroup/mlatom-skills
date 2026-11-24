---
name: omni-p3x-single-point
description: Use OMNI-P3x in MLatom to calculate singlet excitation energies, oscillator strengths, and optional state gradients or neural charges at a fixed molecular geometry.
mlatom: ">=3.28"
tags: [omni-p3x, excited-states, single-point]
---

# OMNI-P3x single-point calculation

## When to use

Use this skill for excited-state properties at a fixed XYZ geometry. For a
broadened absorption spectrum, use the OMNI-P3x UV/vis skill.

## Prerequisites

Use a Python environment with OMNI-P3x-enabled MLatom (introduced in the 3.28
release candidate) and the [aimnetex v0.1.0 backend](https://github.com/mmartyka/aimnetex/tree/v0.1.0).
MLatom downloads the weights on first use. The [MLatom tutorial](https://aitomistic.com/mlatom/tutorial_omnip3x.html)
has a downloadable pyrazine XYZ for a smoke test.

## Steps

1. Use the requested geometry and total charge. Ask for missing geometry or an
   ambiguous charge. The pretrained model covers singlet states of molecules with
   H, B, C, N, O, F, Si, P, S, Cl, Br, and I.
2. Select the user's Python environment. If it lacks OMNI-P3x or its backend,
   report the missing prerequisite. Use the requested model consistently.
3. For excited states, use `TDA-wB97X-D/def2-SVP`. Count S0 in `nstates`, so
   requesting S1–S3 needs `nstates=4`. The supported maximum is S10.

```python
import mlatom as ml

model = ml.models.methods(method="OMNI-P3x")
mol = ml.data.molecule.from_xyz_file("pyrazine.xyz")
mol.charge = 0
model.predict(molecule=mol, nstates=4, level="TDA-wB97X-D/def2-SVP")

print(f"S0: {mol.electronic_states[0].energy:.6f} Hartree")
for state, (gap, strength) in enumerate(
    zip(mol.excitation_energies * ml.constants.Hartree2eV, mol.oscillator_strengths),
    start=1,
):
    print(f"S{state}: {gap:.3f} eV, oscillator strength {strength:.4f}")
```

Add `calculate_energy_gradients=True` to `predict` when requested. Read
`mol.electronic_states[state].energy_gradients` for that state's gradients
in Hartree/angstrom. Read `mol.electronic_states[state].AIMNet_charges` for
its neural charges in units of the elementary charge. For a specified fragment,
sum its atom charges in the excited state and subtract the matching S0 sum.
Preserve the XYZ atom order and label the sign convention. These are model
charges. Report fragment charge differences as such.

## Verify

Check that `len(mol.electronic_states) == nstates`, and that excitation
energies and oscillator strengths both contain `nstates - 1` finite values.
When requested, check that gradients are finite with one three-component row
per atom and that neural charges sum to the molecular charge within numerical
precision. For the tutorial pyrazine geometry, S1–S3 are about 4.145, 4.995, and
5.655 eV.

## Cite

Cite [MLatom](https://github.com/dralgroup/mlatom) and the OMNI-P3x article
linked from the [model tutorial](https://aitomistic.com/mlatom/tutorial_omnip3x.html)
when available. Model weights are on [Zenodo](https://doi.org/10.5281/zenodo.24344107).
