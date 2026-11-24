---
name: omni-p3x-uvvis
description: Simulate a fixed-geometry UV/vis absorption spectrum with OMNI-P3x and MLatom single-point convolution, and save the spectrum and a plot.
mlatom: ">=3.28"
tags: [omni-p3x, excited-states, uvvis]
---

# OMNI-P3x UV/vis absorption

## When to use

Use this skill for a quick UV/vis absorption spectrum from a single geometry
using single-point convolution. Use an explicit ensemble workflow when the user
asks for a nuclear ensemble spectrum.

## Prerequisites

Use an OMNI-P3x-enabled MLatom build (introduced in the 3.28 release candidate),
[aimnetex v0.1.0](https://github.com/mmartyka/aimnetex/tree/v0.1.0), and matplotlib.
The [MLatom tutorial](https://aitomistic.com/mlatom/tutorial_omnip3x.html) provides
a pyrazine XYZ and an example output.

## Steps

1. Use the user's XYZ and total charge. If they request the tutorial example,
   use its pyrazine geometry. Confirm the requested Python environment supports
   OMNI-P3x and the backend. If it lacks either, report the unmet prerequisite.
2. Predict S0 and the requested number of singlet excited states at
   `TDA-wB97X-D/def2-SVP`. For ten excited states, use `nstates=11`.
3. Pass the predicted molecule directly to `ml.spectra.uvvis.spc`. It expects
   the molecule's excitation energies in Hartree. Set the band width in eV.

```python
import mlatom as ml
import numpy as np

model = ml.models.methods(method="OMNI-P3x")
mol = ml.data.molecule.from_xyz_file("pyrazine.xyz")
mol.charge = 0
model.predict(molecule=mol, nstates=11, level="TDA-wB97X-D/def2-SVP")

spectrum = ml.spectra.uvvis.spc(molecule=mol, band_width=0.3)
spectrum.plot(filename="uvvis.png", title="Pyrazine, OMNI-P3x")
np.savetxt(
    "uvvis.csv",
    np.column_stack((spectrum.wavelengths_nm, spectrum.molar_absorbance)),
    delimiter=",",
    header="wavelength_nm,extinction_M-1_cm-1",
    comments="",
)
```

## Verify

Check for finite excitation energies and oscillator strengths, and a finite
spectrum on an increasing wavelength grid. Verify that the CSV and PNG exist
and that at least one transition has positive intensity before reporting a
peak. Give the band width and explain that the output is a calculated spectrum
for the supplied geometry.

## Cite

Cite [MLatom](https://github.com/dralgroup/mlatom) and the OMNI-P3x article
linked from the [model tutorial](https://aitomistic.com/mlatom/tutorial_omnip3x.html)
when available.
