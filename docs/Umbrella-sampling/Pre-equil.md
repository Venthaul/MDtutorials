---
layout: default
title: Pre-equil
parent: Umbrella Sampling
nav_order: 2
---

## Energy minimization and Equilibration

1. Files needed: `step3_input.gro`, `topol.top`, `index.ndx` and `toppar` folder. Rename the .gro file to `system.gro`

2. Find `em.mdp`, `equil.mdp` and `prod.mdp` in folder: `\Open-Close MD\New_exp\Umbrella_sampling`

3. Run Minimization:
```
mkdir EM
cd EM
gmx grompp -f ../em.mdp -o em.tpr -c ../system.gro -r ../system.gro -p ../topol.top -n ../index.ndx -maxwarn 1
gmx mdrun -v -deffnm em -ntmpi 1 -ntomp 16 -nb gpu -gpu_id 0 -pin on
cd ..
```
4. Run Equilibration:
```
mkdir EQUIL
cd EQUIL
gmx grompp -f ../equil.mdp -o equil.tpr -c ../EM/em.gro -r ../system.gro -p ../topol.top -n ../index.ndx -maxwarn 1
gmx mdrun -v -deffnm equil -ntmpi 1 -ntomp 16 -pme gpu -gpu_id 0 -pin on
cd ..
```
5. Prepare index groups [Difficult]
```
gmx trjconv -f EQUIL/equil.gro -n index.ndx -o findcenter.pdb -s EQUIL/equil.tpr
gmx make_ndx -f EQUIL/equil.gro -n index-temp.ndx -o index-temp.ndx
```
It is really important to check the `prep_struc.xlsx` file to find the group we want to create. For example, if we want to create Group `Angle_RBD`, the alpha carbon of residue from 335 to 514 is included. So the command is: (before you try, finish reading this part 5)
```
gmx make_ndx -f EQUIL/equil.gro -o index-temp.ndx
> r 335-514 & a CA [Select alpha carbon of residue from 335 to 514]
> q [save and quit]
```
Then, you can rename the Group name in the `index-temp.ndx` file. If you want to continue to add new Group `Angle_CTD_Hinge`, the command is:
```
gmx make_ndx -f EQUIL/equil.gro -n index-temp.ndx -o index-temp.ndx
> r 321-324 | r 535-582 & a CA
> q
```
However, the residue ID scheme listed in `prep_struc.xlsx` is still WH scheme. Remember that in Glycosylation part, due to the deletions, the ID scheme changed. Same in this part, you need to create the new ID scheme for each group. Finally, copy all the new groups in `index-temp.ndx` and paste them under `index.ndx`.
6. Find each group's center:
When the `index.ndx` is prepared, export each group in PDB format so that we can get the center of each group.
```
gmx trjconv -f EQUIL/equil.gro -n index.ndx -s EQUIL/equil.tpr -o protein_Angle_RBD.pdb
> [select the ID of Angle_RBD]
```
Open the `protein_Angle_RBD.pdb` with Pymol and run `\Open-Close MD\New_exp\COM.py` in Pymol: File --> Run Script...
Then in Pymol's terminal:
```
com all
```
This command will generate a new sphere. Find the nearest atom of the new sphere, which is considered as the center atom of this group. Use sequence viewer to find which residue the center atom belong to and find this atom in `EQUIL/equil.gro` file. Get the atom ID. Then in `prod.mdp` file: (only two groups)
```
pull-group1-pbcatom     = 6172 ; Center atom of Angle_RBD
pull-group2-pbcatom     = 8924 ; Center atom of Angle_CTD_Hinge
```