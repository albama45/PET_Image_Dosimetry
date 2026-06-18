# PET_Image_Dosimetry

# PET Dosimetry Analysis using LegPy Simulations

## Overview
This repository contains materials related to my Bachelor's thesis in Physics at Universidad Complutense de Madrid.

The thesis focus on dosimetric analysis of medical images (PET/CT) using LegPy (Monte Carlo based algorithm) simulations for photon dose deposition. It processes DICOM files, simulates radiation transport, and calculates absorbed dose distributions in tissue.

## Project Structure

```
# Repository Structure

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



## Modules

### 1. DICOM Processing

The `dicom_processing` module contains the tools required to load and process PET and CT DICOM image series.

Main functionalities include:

- Reading DICOM image data.
- Reconstructing 3D volumetric arrays from 2D image slices.
- Extracting relevant physical parameters, such as pixel spacing, slice thickness and image metadata.
- Preparing image data for subsequent activity and dose calculations.

Relevant files:

- `dicom_reader.py`: handles DICOM image loading and reconstruction.
- `obtaining_Activity.py`: extracts or calculates activity-related information from PET DICOM data.

### 2. Dosimetry Calculations

The `dosimetry_calculations` module contains the code used to estimate absorbed dose from PET-derived activity distributions.

Main functionalities include:

- Converting PET image values into activity values.
- Using DICOM rescale factors and radiopharmaceutical information.
- Estimating absorbed dose from activity maps.
- Returning dose values in Gray (Gy).

Relevant file:

- `dose_calculator.py`: performs the main dose calculation workflow.

### 3. Kernel Generation

The `kernel_generation` module contains the tools used to generate and manage dose deposition kernels.

Main functionalities include:

- Generating radiation transport kernels.
- Constructing dose deposition matrices.
- Preparing kernels for convolution with activity maps.
- Saving kernels for later reuse in dosimetry calculations.

Relevant file:

- `kernel_generator.py`: generates and stores dose deposition kernels.

### 4. Testing

The `testing` folder contains scripts used to test the main components of the project, incluiding cross-checks of the results.

Relevant files:


- `test_dosimetry.py`: tests of dose calculation functions; includes the calculation of the dose at specific regions.
- `tests_kernel.py`: tests kernel generation and kernel handling.

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

The repository is organised as a modular workflow for PET image dosimetry. The general process consists of loading PET and CT DICOM image data, extracting activity information, generating or loading a dose deposition kernel, and calculating the absorbed dose distribution.

### 1. DICOM Image Processing

The first step is to load the PET and CT DICOM image series and reconstruct them as 3D volumetric arrays.

```python
from dicom_processing.dicom_reader import *
```

This module is used to:

- Read PET and CT DICOM files.
- Sort 2D DICOM slices.
- Reconstruct volumetric image data.
- Extract image metadata such as pixel spacing, slice thickness and image dimensions.
- Prepare the image data for activity and dose calculations.

### 2. Activity Calculation

The PET image values are converted into activity information using the corresponding DICOM metadata.

```python
from dicom_processing.obtaining_Activity import *
```

This step is used to:

- Convert PET voxel values into activity concentration.
- Apply DICOM rescale factors.
- Extract radiopharmaceutical information from the DICOM header.
- Generate the activity map required for dose calculation.

### 3. Kernel Generation

Dose deposition kernels are generated or loaded before performing the dosimetry calculation.

```python
from kernel_generation.kernel_generator import *
```

This module is used to:

- Generate dose deposition kernels.
- Define voxel dimensions and kernel size.
- Store kernels for later reuse.
- Prepare kernels for convolution with activity maps.

### 4. Dose Calculation

The absorbed dose distribution is calculated by combining the activity map with the dose deposition kernel.

```python
from dosimetry_calculations.dose_calculator import *
```

This step is used to:

- Load or receive the activity distribution.
- Apply the dose deposition kernel.
- Estimate the absorbed dose distribution.
- Return dose values in Gray (Gy).

### 5. Testing

The main components of the project can be tested using the scripts located in the `testing` folder.

```bash
python testing/test_dicom_processing.py
python testing/test_dosimetry.py
python testing/tests_kernel.py
```

These scripts are used to check the DICOM processing, activity calculation, kernel generation and dose calculation modules.

## Required Inputs

The workflow generally requires:

- A PET DICOM image series.
- A CT DICOM image series.
- Relevant DICOM metadata, including voxel dimensions and radiopharmaceutical information.
- A generated or pre-generated dose deposition kernel.

## Output

The main output of the workflow is an estimated absorbed dose distribution expressed in Gray (Gy).

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
