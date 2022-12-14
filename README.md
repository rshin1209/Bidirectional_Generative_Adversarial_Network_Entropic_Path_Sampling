# Bidirectional Generative Adversarial Network - Entropic Path Sampling [Edit on-going]

The repository documents how to perform bidirectional generative adversarial network - entropic path sampling ([BGAN-EPS](https://doi.org/10.26434/chemrxiv-2022-lcfbq)) method developed to accelerate entropic path sampling ([EPS](https://doi.org/10.1021/acs.jpclett.1c03116)) by integrating EPS with deep generative models.
<p align="center">
<img src="https://user-images.githubusercontent.com/25111091/205413472-bf70e899-32f7-4a0c-8dc5-a576c129a36c.jpg" width=75%>
</p>

## Bidirectional Generative Adversarial Network (BGAN)
<p align="center">
<img src="https://user-images.githubusercontent.com/25111091/205412357-c7548b3e-6161-42f6-9c06-3f204374ae7f.jpg" width=50%)
</p>
        
The **bidirectional generative adversarial network (BGAN) model** is designed to enhance the estimation of probability density function of molecular configurations. The BGAN model consists of two pairs of generative adversarial networks (GANs): one is used to generate pseudo-molecular coordinates and the other to generate pseudo-latent variables.

## Module Requirements
- Numpy
- Argparse
- Importlib
- Pytorch
- Sklearn
- Scipy
- Networkx
- Pymol
- GPU Access

## QUICK BGAN-EPS
**For a quick BGAN-EPS test with cyclopentadiene dimerization and NgnD-catalyzed Diels–Alder reaction:**
#### Cyclopentadiene Dimerization
        python bgan_eps.py --filename ./temporary/cp_dimerization/Bond2_1.npy -- topology ./temporary/cp_dimerization/topology.txt --epochs 130 --ensemble 10
        python bgan_eps.py --filename ./temporary/cp_dimerization/Bond3_1.npy -- topology ./temporary/cp_dimerization/topology.txt --epochs 130 --ensemble 10
        
<p align="center">
<img src="https://user-images.githubusercontent.com/25111091/205424656-63bd93e3-3a07-412d-9ed2-57c325f4584c.jpg" width = 50%)
</p>

#### NgnD-catalyzed Diels–Alder reaction
        python bgan_eps.py --filename ./temporary/ngnd_catalyzed_diels_alder/ngnd_64_adduct_postTS.npy -- topology ./temporary/ngnd_catalyzed_diels_alder/topology.txt --epochs 200 --ensemble 9
        python bgan_eps.py --filename ./temporary/ngnd_catalyzed_diels_alder/ngnd_42_adduct_postTS.npy -- topology ./temporary/ngnd_catalyzed_diels_alder/topology.txt --epochs 200 --ensemble 9
        
<p align="center">
<img src = "https://user-images.githubusercontent.com/25111091/205424658-8c148f7c-c478-4faf-9235-181c7fb098e2.jpg" width = 50%)
</p>

*More degrees of freedom require more epochs to fully estimate probability density function.*
        
## How to perform BGAN-EPS
### Step 1: Prepare post-transition-state (post-TS) trajectories and place a single combined file (all post-TS trajectories) in the folder named "dataset" (bond formation cutoff: 1.6 Å for the C-C bond formation).

The dataset for NgnD-catalyzed Diels–Alder reaction in the gas phase are provided in the dataset.
- **6+4 adduct:** ./dataset/ngnd_64_adduct_postTS.xyz
- **4+2 adduct:** ./dataset/ngnd_42_adduct_postTS.xyz

### Step 2: Prepare topology file and convert trajectories from the Cartesian coordinate to the internal coordinate by running the command below.
        python preparation.py --filename ngnd_64_adduct_postTS.xyz --atom1 5 --atom2 14 --atom3 8 --atom4 9
        python preparation.py --filename ngnd_42_adduct_postTS.xyz --atom1 7 --atom2 12 --atom3 8 --atom4 9

The topology file is prepared by representing Cartesian coordinate of reactive species in the graph structure based on the bonding atoms. The connectivity script computes all possible bond, angle, and torsion angle via path finding algorithm and outputs redundant internal coordinates (more than 3N-6) as the connectivity file. Additionally, the user must define the main reacting bond and the first reacting bond. atom1 and atom2 are the atoms involved in the main reacting bond and atom3 and atom4 are the atoms involved in the first reacting bond. If the reaction involves a single bond formation, atom3 and atom4 can be ignored.

### Step 3: Run bgan_eps.py to evaluate the entropic profiles by running the command below.

        python bgan_eps.py --filename ./temporary/ngnd_64_adduct_postTS.npy --epochs 200 --ensemble 9
        python bgan_eps.py --filename ./temporary/ngnd_42_adduct_postTS.npy --epochs 200 --ensemble 9

### BGAN-EPS output example
        Epoch [199] Time [447.8589] g_loss [3.3698] h_loss [3.4197] g_h_loss [3.7789] dx_loss [0.2072] dy_loss [0.1925] d_loss [0.3997]
        [2.8801820405583136, 2.7586166619128925, 2.6229387346696513, 2.4697832319802027, 2.3132930670672986, 2.1556491449613544, 1.9972661171376582, 1.8513560794384587, 1.7299191231963564]
        [137.55619703486667, 129.01194725070548, 127.87302142008913, 124.71919021784925, 124.50773301633102, 125.00839534670577, 125.96119163643706, 126.82494799247753, 132.31009158720974]

- BGAN output at each epoch
- The main reacting bond length of structural ensembles
- The entropy value for each structural ensemble in kcal/mol at 298.15 Kelvin

## Contact
Please feel free to open an issue in Github or directly contact wook.shin@vanderbilt.edu if you have any problem in BGAN-EPS.

## Citation
Shin W, Ran X, Wang X, Yang Z. Accelerated Entropic Path Sampling Elucidates Entropic Effects in Mediating the Ambimodal Selectivity of NgnD-Catalyzed Diels–Alder Reaction. ChemRxiv. Cambridge: Cambridge Open Engage; 2022;  This content is a preprint and has not been peer-reviewed.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
