# PET_Image_Dosimetry

# PET Dosimetry Analysis using Monte Carlo Simulations

## Overview

This project performs dosimetric analysis of medical images (PET/CT) using Monte Carlo simulations for photon dose deposition. It processes DICOM files, simulates radiation transport, and calculates absorbed dose distributions in tissue.

## Project Structure

```
# Recommended Repository Structure

```plaintext
PET_Image_Dosimetry/
│
├── dicom_processing/
│   ├── __init__.py
│   ├── dicom_reader.py
│   └── obtaining_Activity.py
│  
│
├── kernel_generation/
│   ├── __init__.py
│   ├── kernel_generator.py
│   └── utils.py
│

├── dosimetry_calculations/
│   ├── __init__.py
│   ├── dose_calculator.py
│   └── utils.py
│
├── tests/
│   ├── test_kernel.py
│   ├── test_dicom_processing.py
│   └── test_dosimetry.py
│
├── requirements.txt
└── README.md
```

## Features

### Main Script (`version3.py`)

1. **DICOM Image Processing**
   - Loads PET and CT image series from DICOM files
   - Reconstructs 3D volumetric data from 2D slices
   - Extracts physical parameters (pixel spacing, slice thickness)

2. **Activity Calculation**
   - Converts raw pixel values to activity (Bq/ml) using DICOM rescale factors
   - Handles radiopharmaceutical information (isotope identification)

3. **Dose Deposition**
   - Convolution of activity maps with radiation transport kernels
   - Accounts for photon and alpha particle contributions
   - Outputs dose in Gray (Gy)

4. **Visualization**
   - Multi-plane visualization (axial, coronal, sagittal)
   - Linear and logarithmic scaling options
   - Anatomical region zoom capabilities

### Kernel Generation (`Obtencion_Kernel_limpio.ipynb`)

- Monte Carlo simulations using LegPy (Legendre-based physics simulation)
- Simulates photon transport from positron-electron annihilation
- Generates dose deposition matrices for isotropic point sources
- Saves kernels as pickle files for reuse

## Dependencies

```python
pydicom          # DICOM file handling
numpy            # Numerical computations
matplotlib       # Visualization
scipy            # Image processing (gaussian filtering)
SimpleITK        # Medical image processing (optional)
pickle           # Data serialization
LegPy            # Monte Carlo simulations (git clone)
```

## Usage

### 1. Kernel Generation

```bash
jupyter notebook Obtencion_Kernel_limpio.ipynb
```

Key parameters:
- `Ladox`, `Ladoy`, `Ladoz`: Voxel dimensions (cm)
- `Nx`, `Ny`, `Nz`: Number of voxels per dimension
- `n_part`: Number of photon histories (10M recommended for accuracy)

Output: `Kernel_fotones_2.pkl` containing dose deposition matrix

### 2. Dosimetry Analysis

```bash
python version3.py
```

Required inputs:
- PET DICOM series (zip file: `DatosPET1.zip`)
- CT DICOM series (zip file: `DatosCT.zip`)
- Pre-generated kernel file (`Kernel_fotones_2.pkl`)

## Key Physical Concepts

### Activity to Dose Conversion

Total dose calculated as:
```
Dose = Dose_photons + Dose_alphas
Dose_photons = Conv(Activity, K_photons)  # Convolution with kernel
Dose_alphas = a × Activity                 # Direct contribution (a = 1.6×10⁻¹⁰)
```

### Kernel Representation

The kernel K(r) represents:
- Energy deposited per history per voxel (MeV/cm³)
- Radially symmetric for isotropic sources
- Obtained from Monte Carlo simulation in water

## Output Analysis

### Dose Profiles
- Radial energy profiles from center voxel
- Linear and logarithmic scale representations
- Identification of critical organs (bladder, brain, lungs)

### Statistics
- Maximum dose locations
- Regional dose averages
- Comparison with reference values

## Example Results

```
Organ          Dose (mGy)   Reference
Bladder        15 ± 2       20 ± 3
Brain          0.042        0.042*
Lungs          Variable     Depends on uptake
```

## Notes

- **Units**: Activity in Bq/ml, Dose in Gy (SI standard)
- **Accuracy**: Improves with Monte Carlo statistics (higher n_part)
- **Computation**: Monte Carlo simulations are time-intensive (~6.3×10⁻⁵ s per history)
- **Geometry**: Current implementation assumes water medium

## Future Improvements

- [ ] Tissue-specific attenuation (HU to material conversion)
- [ ] Multiple isotope support (not just ¹⁸F)
- [ ] GPU-accelerated convolution
- [ ] Organ segmentation and selective dose reporting
- [ ] Uncertainty quantification

## References

- Medical Imaging: DICOM Standard
- Radiation Transport: Monte Carlo methods
- Dosimetry: ICRU Report 60 (Fundamental Quantities for Use in Radiotherapy)

## Authors

Alba M. (dosimetry analysis)
Contributors welcome!

## License

[To be specified]

---

**Last Updated**: February 2025
**Status**: Development
</final_output>
