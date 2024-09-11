---
layout: default
title: Prepare system
parent: Umbrella Sampling
nav_order: 1
---

## Build fully glycosylated Spike protein

1. Download --> Wentao's share folder: `\Open-Close MD\New_exp\Delta_woGlycan.pdb`.

2. Go to the [CHARMM-GUI webserver](https://www.charmm-gui.org/?doc=input).

3. Upload Delta_woGlycan.pdb to [Input Generator\Glycan Reader & Modeler](https://www.charmm-gui.org/?doc=input/glycan).

4. Go to Manipulate PDB page, this step may take some time since our structure is really big.

## Find glycosylation sites and add glycan

Due to the deletion, the residue ID of the input structure is mismatched. We should follow the WH variant's residue ID scheme.
{: .fs- .fw-300 }

1. Go to `\Open-Close MD\New_exp\prep_structure.xlsx` and open the Delta_glycan_sites sheet.

2. Go to `\Open-Close MD\New_exp\Glycan_CHARMMGUI.pptx`, for each site's glycosylation situation, detailed information are provided.

3. In CHARMM-GUI page, turn on Glycosylation and click "Add Glycosylation" and "edit" buttoms to add glycan on specific sites.

    1. In "edit" window, under the "Glycan Sequence", choose the "ASN" as residue type and the right sites based on `prep_structure.xlsx`.
    2. Click the "Upload GRS" and paste the GRS recorded in `Glycan_CHARMMGUI.pptx`, then click "Apply". 
    3. Once the Sequence Graph is consistant with the graph shown in the PPT, click "Next Step". 
    4. Each sites recorded in `prep_structure.xlsx` should be glycosylated.
    {: .fs-3 .fw-400 }

4. Go to Generate PDB page.

## Solvation and forcefield

1. In Waterbox Size Options, choose "Fit Waterbox Size to Protein Size" and set type as Rectangular.

2. In "Enter Edge Distance" box, use "20.0" instead of "10.0", then click "Calculate Solvent Composition" buttom.

3. Go to Solvate Molecule page.

4. Choose CHARMM Forcefield and tick the GROMACS as output.