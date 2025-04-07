---
layout: post
title: Molecular Dynamics Simulation
date:   2025-02-03 10:11:50 +0300
img: 20.jpg
tags: [Drug Discovery, Molecular Dynamics]
---
# MD Simulation

Molecular docking has been a game changer in drug discovery, allowing us to predict how a small molecule (our potential drug) fits into a protein, like a key sliding into a lock. It gives us a static snapshot of the binding affinity, a single frozen moment and suggests **Yes, this molecule might work**.

But, nature doesn’t work in frozen frames. Proteins are not rigid structures, proteins are not still-life paintings. Biological molecules are in constant motion, fluctuating, twisting and turning themselves in response to their surroundings. A molecule that *looks* like a perfect fit in docking might be unstable in reality-drifting away, falling apart making binding impossible over time. 

This is where Molecular Dynamics (MD) Simulation comes in. Instead of a single snapshot, what if we got a movie; a step by step evolution of molecular interactions over time allowing us to simulate how molecules behave in their natural, dynamic state, capturing subtle sifts in structure and true stability of a bound complex. 

So, let’s go beyond the static world of docking and simulate the dynamic dance of molecules. 

But first, let’s clear some topics that are imperative to MD Simulation. I’ll lay out just the names for now and connect them for MD Simulation. 

### Sub-Topics

1. Force Field
2. Energy Minimization
3. Periodic Boundary Conditions (PBC)
4. Neighbor Lists & Cutoffs
5. Electrostatics
6. Thermodynamic Ensemble
7. Integrator
8. Constraint Algorithms
9. Timestep & Simulation Length
10. Thermostat
11. Barostat

Daunting, right ? So, let’s see how we can simplify it and understand MD simulation on an intuitive level.

## Imagining a World Without Motion

Let’s start at the very beginning. Imagine an empty void, a vaccum where time has no meaning. Floating in space is a protein—a vast, intricate structure, silent and unmoving. Nearby, a ligand drifts aimlessly, awating an interaction. 

But, how do we define this world ? What parts of nature should we keep? What parts should we ignore? 

At this moment, our universe lacks forces, motion, and space itself. We must carve out the essential laws that govern our molecules. 

## Forces: The Invisible Threads that Bind

For our simulation to feel real, we need to understand what holds molecules together. The protein and ligand are not truly lifeless—they push and pull against each other with invisible threads of force: 

- Some forces pull atoms together, keeping bonds intact, angles firm, and torsions resisting change.
- Others are more subtle—the quiet shispers of electrostatics, where opposite charges attract and like charges repel.
- And then there are soft brushes of contact, the weak yet persistent van der Waals interactions that define how snugly molecules fit into each other’s contours.

But something still feels wrong. 

This isn’t how molecules exist in the real world. 

## Building a World That Matters

In nature, proteins do not float in an empty vaccum—they are surrounded by water, jostled by molecular chaos, twisted by the motions of heat. 

So, let’s carve out a world, but not an entire universe—just enough to capture what matters. 

First, we place our system inside a box—a controlled space where we can track everything within. But there’s a problem: molecules don’t stop at walls in reality. A protein in water isn’t trapped in a rigid boundary. 

To fix this, we let space wrap around itself, like the snake game where exiting one side brings you back on the other. This creates an illusion—a Periodic Boundary Condition (PBC)—that makes our tiny system behave as if it were a part of something much larger. 

Now, our molecules are free to move naturally, just as they would in an infinite medium. 

## Time and the Dance of Motion

Now that we have created a world, we must bring it to life. 

- How fast should the molecules move ?
- How do they tremble with heat ?
- How do we ensure they behave as they would in nature ?

A thermostat gently guides temperature, ensuring that movement reflects the reality of a biological system. Similarly, a barostat controls pressure—squeeze too hard, and structures collapse; relax too much, and molecules drift apart unnaturally. These forces keep our universe stable. 

But something is still missing: time itself. 

In the molecular world, time is measured in femtoseconds—tiny fragments where atoms vibrate, shift, and respond to forces. But we cannot watch this motion all at once, so we record frames, tiny increments where we calculate new positions based on the forces acting at every moment. 

This is the role of an integrator, solving Newton’s laws again and again, ensuring that motion flows smoothly, one timestep at a time. 

## The Last Step: Preparing for Motion

Even with everything in place, nature remains restless. The starting positions of our molecules might have unnatural strain—atoms too close together, artificial clashes that disrupt the harmony of motion.

Before setting things in motion, we must first relax the system—allowing it to find a stable, low-energy state before the simulation begins. This is energy minimization, the deep breath before the dance, the moment where chaos softens into harmony, ready for the unfolding story of movement.

## The Story Begins

Now, everything is set. 

We have built a world, defined its laws, set forces in motion and prepared the system for its first steps. 

From here, the simulation unfolds, and we watch.

Now, we will dive into each topic, one by one, mathematically and how we will use them with OpenMM

## Force Fields

A simulation can only be as good as the forces governing the system. At the heart is the forcefield, a mathematical framework that defines how atoms interact based on classical physics. A forcefield is not a physical entity but a set of mathematical equations that describe:

1. Bonded interactions: Forces acting within a molecule
2. Non-bonded interactions: Forces between separate molecules or distant atoms 

So, general form for the total energy in an additive force field can be written as:

$$
E_{total} = E_{bonded} + E_{nonbonded}
$$

Where, the components of the covalent and noncovalent contributions are given by the following simulations:

$$
E_{bonded} = E_{bond} + E_{angle} + E_{dihedral}
$$

$$
E_{nonbonded} = E_{electrostatic} + E_{van\ der\ Waals}
$$

Let’s break down the forces:

### Bonded Interactions:

**Bond Potential**: Bonds between atoms behave like springs. THe energy required to stretch or compress a bond(Hooke’s Law) is:

$$
U_{bond} = \sum_{bonds}{k_b(r_{ij} - r_0)^2}
$$

where:

- $k_b$ is the force constant
- $r_{ij}$ is the bond length
- $r_0$ is the value for bond length betweens atoms $i$ and $j$ when all other terms in the force fields are set to 0

Through this, a reasonable accuracy at bond lengths near equilibrium distance is provided, it is less accurate as one moves away. For most practical applications, this is good enough.

*How long the bond is and how much energy does it take to stretch or compress it?*

**Angle Potential:** The bond angles also follow a harmonic potential (bending):

$$
U_{angle} = \sum_{angles}{k_{\theta}(\theta - \theta_0)^2}
$$

where:

- $k_{\theta}$ is the angle force constant
- $\theta$ is the current bond angle
- $\theta_0$ is the equilibrium bond angle

*How much does the bond angle bend and how much energy is needed to bend it ?*

**Dihedral Potential(Torsional Rotation)**

The energy for rotating around a bond is given by a periodic function (twisting):

 

$$
U_{torsions} = \sum_{torsions}{k_{\phi}(1 + cos(n\phi - \gamma))}
$$

where:

- $k_\phi$ is the torsion force constant
- $\phi$ is the torsion angle
- $n$ is periodicity
- $\gamma$ is the phase angle

*How much does one part of a molecule can rotate relative to another, and  what energy barriers exist for rotation?* 

So, using these three equations, we first keep atoms at a reasonable distance, prevent them from unnatural bending and stop the molecules from spinning freely and enforce realistic rotations.

### Non-Bonded Interactions:

**Electrostatic Interactions**

As the atoms themselves are charged particles, they interact with each other via electric forces. Using Coulomb’s law:

$$
U_{electrostatics} = \frac{1}{4\pi\epsilon}\sum_{i<j}{\frac{q_iq_j}{r_{ij}}}
$$

where:

- $q_i,q_j$ are charges on atoms $i$ and $j$
- $r_{ij}$ is the distance between atoms
- $\epsilon$ is the dielectric constant

**van der Waals Interactions**

The van der Waals interactions have an origin from quantum mechanical effects but for computationally efficient approximation, we use Lennard-Jones Potential

The van der Waals force is a distance dependent interaction between atoms or molecules. These attractions are comparitively weak and therefore more susceptible to disturbance. The van der Waals force quickly vanishes at longer distances between interacting molecules.

Instead of using a complicated formula, vdW interactions are modelled with the Lennard-Jones 12-6 potential:

$$
U_{LJ}(r) = 4\epsilon[(\frac{\sigma}{r})^12-(\frac{\sigma}{r})^6]
$$

where:

- $\epsilon$ is the depth of the potential well
- $\sigma$ is the distance where the potential is zero
- $r^{-12}$ approximates repulsion (Pauli’s exclusion)
- $r^{-6}$ models attraction (London dispersion)

Now, we have all the forces.. everything should be easy, right? Well, not. In an ideal world, we would be solving Schrodinger’s equation for every atom, but it’s computationally impossible. So, the constants that we had been defining are sometimes derived from experimental data, sometimes from quantum mechanics, and sometimes with Emperical knowledge. Therefore resulting in many forcefields, each tuned for it’s own case. Some force fields might be useful for Proteins, and we might need a different force field for small organic molecules. Some force fields are AMBER, CHARMM, OPLS-AA, etc, just to name a few.

### In OpenMM:

```python
from openmm.app import *
from openmm import *
#Loading the Amber Force Field 
curr_forcefield = ForceField("amber14-all.xml", "amber14/tip3pfb.xml")
#Btw, you can define your own force field: http://docs.openmm.org/7.6.0/userguide/application/05_creating_ffs.html
```

References (Force Fields):

1. [https://computecanada.github.io/molmodsim-md-theory-lesson-novice/01-Force_Fields_and_Interactions/index.html](https://computecanada.github.io/molmodsim-md-theory-lesson-novice/01-Force_Fields_and_Interactions/index.html) 
2. [https://en.wikipedia.org/wiki/Force_field_(chemistry)](https://en.wikipedia.org/wiki/Force_field_(chemistry)) 
3. [https://en.wikipedia.org/wiki/Van_der_Waals_force#:~:text=8 External links-,Definition,a consequence of quantum dynamics](https://en.wikipedia.org/wiki/Van_der_Waals_force#:~:text=8%20External%20links-,Definition,a%20consequence%20of%20quantum%20dynamics). 
4. [https://en.wikipedia.org/wiki/Lennard-Jones_potential](https://en.wikipedia.org/wiki/Lennard-Jones_potential)

## Energy Minimization

Energy minimization is a technique used to find the most stable arrangement of atoms in a molecular system by minimizing its potential energy. This step is done while preparing molecular structures before performing dynamics simulations.

For example, when optimizing the geometry of a water molecule, the aim is to obtain the hydrogen-oxygen bond lengths and the hydrogen-oxygen-hydrogen bond angle which minimize the forces that would otherwise be pulling atoms together of pushing them apart.

(*Why? :Nature is most often Optimized)*

It works iteratively by reducing the total potential energy of the system.

$$
U_{total} = U_{bonded} + U_{non-bonded}
$$

**Mathematically, we want to find a configuration where:**

$$
\frac{dU}{dr} = 0
$$

where $U$ is the potential energy, $r$ represents atomic positions.

So, we are searching for a “local minimum” in the energy landscape, where small movements do not lead to drastic energy changes. 

**OpenMM uses L-BFGS (Limited-memory Broyden-Fletcher-Goldfarb-Shanno) Algorithm**

Let’s see how we can minimize energy in OpenMM

```python
from openmm import *
from openmm.app import *
from openmm.unit import *

# Load system
pdb = PDBFile('protein_ligand.pdb')
forcefield = ForceField('amber14-all.xml', 'amber14/tip3p.xml')

# Create system with force field
system = forcefield.createSystem(pdb.topology, nonbondedMethod=PME,
                                 nonbondedCutoff=1.0*nanometer, constraints=HBonds)

# Integrator (dummy, as we only minimize)
integrator = LangevinIntegrator(300*kelvin, 1/picosecond, 0.002*picoseconds)

# Set up simulation
simulation = Simulation(pdb.topology, system, integrator)
simulation.context.setPositions(pdb.positions)

# Run energy minimization
print("Minimizing energy...")
simulation.minimizeEnergy(maxIterations=1000)
print("Minimization complete.")
```

References (Energy Minimization):

1. https://www.nature.com/articles/s42004-024-01114-5
2. [https://en.wikipedia.org/wiki/Energy_minimization](https://en.wikipedia.org/wiki/Energy_minimization)
3. [https://en.wikipedia.org/wiki/Limited-memory_BFGS](https://en.wikipedia.org/wiki/Limited-memory_BFGS) 

## Periodic Boundary Conditions (PBC)

We need to simulate a small system while making it behave like an infinite one. Without them, the molecules will hit the walls of the simulation box and artificially stop moving, the system would feel unnaturally small, leading to surface effects.

**Mathematically**

The system is wrapped around a cubic box with dimensions $L_x, L_y, L_z$. If an atom moves beyond the boundary:

$$
r_i = r_i - L \times round(\frac{r_i}{L})
$$

where:

- $r_i$ is the atom’s position
- $L$ is the box length
- `round` ensures the atom wraps around correctly to the other side

So, at the boundary:

1. Interactions are computed using the closest periodic image
2. Minimum image convention is used—An atom interacts with the closest periodic copy of another atom. 

In OpenMM

```python
# Define a system with PBC
system = forcefield.createSystem(
    pdb.topology,
    nonbondedMethod=PME,  # Enables periodic electrostatics
    nonbondedCutoff=1.0*nanometer,  # Defines interaction range
    constraints=HBonds
)
```

Checking the box Size:

```python
# Get simulation box dimensions
box_vectors = simulation.context.getState().getPeriodicBoxVectors()
print("Box dimensions:", box_vectors)

```

Neighbor Lists and CutOffs are automatically handled when defining the nonbonded interactions. 

Why ? Calculating every possible pairwise interaction would be billions of force calculations per step. So, beyond a certain distance, forces like van der Waals and electrostatic forces (that decay with distance) don’t contribute much.

```python
system = forcefield.createSystem(
    pdb.topology,
    nonbondedMethod=PME,  # Enables Ewald summation
    nonbondedCutoff=1.0*nanometer  # Cutoff for direct-space interactions
)

```

## Electrostatics

*Already written in previous section, need to manage*

## Thermodynamic Ensemble

Now that we have defined forces, periodic boundaries, and electrostatics, we need to decide how to control temperature and pressure during our simulation.

A thermodynamic ensemble is a set of conditions that define how energy, temperature, volume and pressure are controlled in a molecular dynamics (MD) simulation.

**Types Of Thermodynamic Ensembles Used in MD**

| **Ensemble** | **Fixed Quantities** | **What it Simulates** |
| --- | --- | --- |
| NVE(Microcannonical) | Energy *E,* Volume *V,* Number of Particles *N* | Isolated system (no heat or pressure exchange) |
| NVT(Canonical) | Temperature *T,* Volume *V,* Number of particles *N* | Constant-temperature system (solvent in a fixed box) |
| NPT(Isothermal-Isobaric) | Temperature *T,* Pressure *P,* Number of Particles *N* | System under physiological conditions (biological realism) |

Let’s discuss more on Ensembles: 

**NVE Ensemble:** 

- Energy is conserved, meaning no heat or pressure is exchanged
- **Not ideal for biomolecular simulations**, because real proteins and water exchange heat with their surroundings.

```python
integrator = VerletIntegrator(0.002*picoseconds)  # No temperature control
```

**NVT Ensemble:**

- Keeps temperature constant using a thermostat
- Volume is fixed → No pressure regulation
- Used in short simulation where we need to maintain a controlled temperature

```python
integrator = LangevinIntegrator(
    300*kelvin,  # Target temperature
    1/picosecond,  # Friction coefficient (damping)
    0.002*picoseconds  # Time step
)
```

**NPT Ensemble**

- Keeps temperature and pressure constant using both a thermostat and a barostat
- Allows the simulation box to adjust in size → More realistic for biological simulations
- Used in most biomolecular studies (proteins in a solution, drug binding)

```python
system.addForce(MonteCarloBarostat(1*bar, 300*kelvin, 25))  # Pressure control
```

## Integrator: Advancing the system in Time:

Now that we’ve set up forces, electrostatics, periodic boundaries, and thermodynamic control, we need to evolve the system in time. This is done using an integrator. 

**Why an Integrator**

MD Simulations follow Newton’s Second Law:

$$
F = ma
$$

which in terms of potential energy $U(r)$ becoms:

$$
m\frac{d^2r}{dt^2} = -\nabla U
$$

This is a second order differential equation that we cannot solve analytically for complex systems, so we use numerical integration to approximate how atoms move. 

The integrator updates 

1. Positions $(r)$
2. Velocities $(v)$
3. Accelerations $(a)$

at each timestep.

### A Simulation Example:

*This simulation example will provide as less technical details as possible. The basics are the same, with just OpenMM flavor.* 

**Only the bits and pieces and important code are given here, proper running code (”press enter and run”) code can be found at:** [https://github.com/chinge55/openmm_practice/tree/master](https://github.com/chinge55/openmm_practice/tree/master) 

Now, I assume you have the following things:

1. Docked ligand pose `.pdbqt`  file.
2. Original Ligand `.sdf`  file.
3. Prepared protein `.pdb`  file. 

First, let’s understand why these files are necessary. So, I’ll just walk through the process. 

The `pdbqt`  file basically doesn’t contain bond information explicitly but contains 3D coordinates of the atoms. If we just try to load the `pdbqt`  file, or just convert it to `pdb`  and load it, the bond information can be incorrect (mostly is). This will cause problems. 

So, what we want to do it take the coordinates from the docked ligand and take the bond information from the original ligand file. Doing this, the bonds, valencies etc will everything be correct while retaining the docked information. 

For the examples below:

- Protein (2B7A)
- Ligand (Pubchem- 66646982)

1. Convert the `pdbqt`  file to `pdb`  file. *(Note that bond information is not correct still)*

```bash
$ obabel out.pdbqt -O ligand_docked.pdb -d
```

1. Now, the original `sdf`  file has unnecessary information as well. So, to match it with what we have. 

```bash
$ obabel ligand_pubchem-d -O ligand_original.sdf -d
```

*We had just removed hydrogens from both, well who cares about hydrogen anyway.* 

1. Now, transferring coordinates from docked file to original.

```python
from rdkit import Chem
from rdkit.Chem import AllChem

# Load the original molecule (which has correct bonds)
original_mol = Chem.MolFromMolFile("ligand_original.sdf")

# Load the docked molecule (without sanitization)
docked_mol = Chem.MolFromPDBFile("ligand_docked.pdb", sanitize=False)

if original_mol.GetNumAtoms() != docked_mol.GetNumAtoms():
    raise ValueError("Mismatch in the number of atoms between original and docked ligand.")

conf = docked_mol.GetConformer()
new_conf = Chem.Conformer(original_mol.GetNumAtoms())

for i, atom in enumerate(original_mol.GetAtoms()):
    pos = conf.GetAtomPosition(i)
    new_conf.SetAtomPosition(i, pos)

original_mol.RemoveAllConformers()
original_mol.AddConformer(new_conf)

Chem.MolToMolFile(original_mol, "ligand_final.sdf")

print("Successfully transferred coordinates! Saved as ligand_final.sdf")
```

**Ligand Preparation**

Notice that I had not added hydrogen previously. So, I will add hydrogens now So, we do that in the ligand that was saved in the previous step.

## References (General):

1. [https://pmc.ncbi.nlm.nih.gov/articles/PMC6209097/](https://pmc.ncbi.nlm.nih.gov/articles/PMC6209097/) 
2. [https://computecanada.github.io/molmodsim-md-theory-lesson-novice/](https://computecanada.github.io/molmodsim-md-theory-lesson-novice/)
