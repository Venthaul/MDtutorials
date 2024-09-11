---
layout: default
title: Pulling
parent: Umbrella Sampling
nav_order: 3
---

## Pulling codes

1. Start run pulling:
```
mkdir Pull_round1
cd Pull_round1
gmx grompp -f ../prod.mdp -o prod.tpr -c ../EQUIL/equil.gro -r ../EQUIL/equil.gro -p ../topol.top -n ../index.ndx
gmx mdrun -deffnm prod -pf pullf.xvg -px pullx.xvg -ntmpi 1 -ntomp 16 -pme gpu -gpu_id 0 -pin on -pinstride 1 -pinoffset 0
```
2. For the pulling result, we want to check if the last structure of the Pulling trajectory is really the ideal "open state" of spike protein. So, we need to extract the last structure of the trajectory:
```
gmx trjconv -f prod.gro -o prod-round1.pdb -s prod.tpr -n ../index.ndx
> [The ID of SOLV]
```
Then open the `prod-round1.pdb` in Pymol and make an alignment with `\Open-Close MD\New_exp\6vsb.pdb`.
3. If the structure cannnot align well, you need to change the pulling code in `prod.mdp`:
```
pull-coord1-rate        = 0.055   ; deg/ps
pull-coord2-rate        = 0.06    ; deg/ps
```
You can just have a try to see how these two parameter influence the motion of RBD. This step really need patience. Try several times to find the best trajectory. Name the hit trajectory `HIT`.
4. Once you get a good pulling trajectory of spike protein, we need to generate the conformations based on the trajectory:
```
mkdir conf
gmx trjconv -s prod.tpr -f prod.xtc -n index.ndx -o conf/conf.gro -sep
```