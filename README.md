# WIANA-beta.1

![License](https://img.shields.io/badge/license-BSD--3--Clause--Clear-yellow)

This is a tool for non-covalent interaction analysis in both periodic and molecular systems. It is currently a preliminary version, with calculation results validated against published data. The source code will be released in a future update once the program is further refined.

---

## ✨ Supported Methods

- **NCI** (Non-Covalent Interactions Index)
- **DORI** (Density Overlap Regions Indicator)
- **IRI** (Interaction Region Indicator)

---

## 📂 Supported Charge Density Formats

| Format | Source Software | Description |
| :---: | :--- | :--- |
| CHGCAR | VASP | Directly read VASP output charge density files |
| XSF | Quantum ESPRESSO, ABINIT | Support XSF format output from QE and ABINIT |
| CUBE | Gaussian | Supports CUBE format output from Gaussian |

---

## 🔧 Installation

The program was developed in **MATLAB 2022a** and is distributed as compiled executable files. Users **do not need to install the complete MATLAB environment**; the required **free MATLAB Runtime toolkit** will be installed automatically by the installer.

The source code will be released once the program is further refined.

### Download Versions

Currently, two versions of the executable are provided:

| Version | Description | Tested On | Executable Path |
| :--- | :--- | :--- | :--- |
| WIANA_b1_Installer_win_web.exe | MATLAB Compiler Standalone Application Installer for Windows | Windows 10 (64-bit) | `<Installation_Path>\application\WIANA_b1.exe` |
| WIANA_b1_linux_web.install | MATLAB Compiler Standalone Application Installer for Linux | Ubuntu 20.04 LTS (64-bit) | `<Installation_Path>/application/WIANA_b1` |

---

## 🚀 Usage Example

### On Windows

**Step 1: Prepare Input Files**
The following files need to be prepared before running:

- Program `WIANA_b1.exe`
- Input file `wiana.ini` (text format containing calculation parameters)
- Charge density file

**Step 2: Run the Program**
Run the program `WIANA_b1.exe`.

**Step 3: View Results**
Calculation results are saved in the automatically generated `Results_Output` folder.

### On Linux

To ensure the program correctly loads the required dynamic libraries, it is recommended to launch it via the provided Shell script `run_WIANA_B_Linux.sh`. The launch script is located in the `<Installation_Path>/application`. The remaining steps are identical to those on Windows.

---

## 📝 Simplified `wiana.ini` File

```ini
FilePath = PATH\CHGCAR         # Charge density path
CalcType = RDG, IRI, DORI      # Calculation methods
AlgoType = 1                   # Algorithm: default resolution mode
Results_Output = result        # Output path
SignRhoMin = -inf              # No sign(λ₂)ρ range filtering
SignRhoMax = inf
SaveScatter = 0                # Save scatter plot, recommended to be set to 0 for large systems.
```

---

## ⚙️ Input Parameters Description

### Required Parameters

#### `FilePath`

- **Type**: String
- **Default**: None
- **Description**: Specifies the path to the charge density file. Both absolute and relative paths are supported. The program automatically determines the data format from the filename (e.g., CHGCAR, XSF, CUBE).
- **Example**: `FilePath = E:\Matlab\CHGCAR_KDP`

### Optional Parameters

The following parameters are optional settings. The program provides reasonable default values, which users can adjust according to calculation requirements.

#### `CalcType`

- **Type**: String
- **Default**: RDG
- **Description**: Specifies the scalar fields to be calculated. Multiple fields can be selected simultaneously, separated by commas or spaces. Supported values include:
  - `RDG` - Reduced Density Gradient function (for NCI analysis)
  - `IRI` - Interaction Region Indicator function
  - `DORI` - Density Overlap Regions Indicator
- **Example**: `CalcType = RDG, IRI, DORI`

#### `AlgoType`

- **Type**: Integer
- **Default**: 1
- **Description**: Selects the resolution used to calculate the gradient. Set AlgoType = 1 for the default resolution mode. Set AlgoType = 2 for the high resolution mode.
- **Example**: `AlgoType = 1`

#### `Results_Output`

- **Type**: String
- **Default**: Results

- **Description**: Specifies the output folder. Both absolute and relative paths are supported. If the specified folder does not exist, the program creates a default folder in the current directory.
- **Example**: `Results_Output = Results`

#### `SignRhoMin/SignRhoMax`

- **Type**: Float
- **Default**: -inf/inf
- **Description**: These two parameters specify the range of sign(λ₂)ρ. When both parameters are set to finite values, the program keeps only data points within this range for analysis and output. This allows users to focus on specific interaction regions.
- **Example**: `SignRhoMin = -0.03`, `SignRhoMax = 0.01`

#### `SaveScatter`

- **Type**: Integer
- **Default**: 1
- **Description**: This parameter controls whether scatter plot images are exported. Set to 1 to export, or 0 to not export. The exported images are PNG files. For limited memory or large systems, setting `SaveScatter = 0` is recommended. The plot can then be generated using the data from the `*_Full_SignRho.dat` file.
- **Example**: `SaveScatter = 0`

Additional parameters are documented in the user manual.

---

## 📊 Output File Description

### Output Folder Naming Convention

All results are saved in automatically generated folders under the root directory specified by `Results_Output`. The folder naming convention is:
{CalcType}_{AlgoStr}_{FileType}_{MatName}

Where:

- `CalcType`: Calculation types connected with underscores (e.g., `RDG_IRI`)
- `AlgoStr`: `Direct` or `Interp` (corresponding to AlgoType)
- `FileType`: Input file type (e.g., `VASP`, `XSF`, `CUBE`)
- `MatName`: Material name extracted from the input filename (removing common prefixes such as `CHGCAR_`)

> **Note**: If the folder already exists, suffixes like `_1`, `_2` will be automatically added.

### Output File Details

Taking the calculation of `RDG` with the input file `CHGCAR_KBBF` as an example, the output directory `Results_Output/RDG_Direct_VASP_KBBF/` contains the following files:

| File Name | Description | Purpose |
| :--- | :--- | :--- |
| `KBBF_RDG.vasp` | Contains RDG scalar field data in CHGCAR format | Various plotting analyses (e.g., isosurfaces via VESTA/XcrySDen) |
| `KBBF_RDG_Full_SignRho.dat` | Text file containing three columns: sign(λ₂)ρ, RDG, and the color mapping quantity | Drawing scatter plots |
| `KBBF_sign_rho.vasp` | Contains sign(λ₂)ρ scalar field data in CHGCAR format | Applying color mapping to the plot |
| `RDG_Scatter.png` | A scatter plot where the horizontal and vertical axes represent sign(λ₂)ρ and RDG, respectively | Conducting preliminary analysis |
| `wiana.log` | Contains information generated during program execution | Checking the running status of the program |

---

## 📚 Citation

When using corresponding methods for structure analysis, authors are recommended to cite the original literature where the methods were published:

- **NCI**: E. R. Johnson, S. Keinan, P. Mori-Sánchez, J. Contreras-García, A. J. Cohen, W. Yang, _J. Am. Chem. Soc._ 2010, **132**, 6498-6506. DOI: [10.1021/ja100936w](https://doi.org/10.1021/ja100936w)

- **DORI**: P. de Silva, C. Corminboeuf, _J. Chem. Theory Comput._ 2014, **10**, 3745. DOI: [10.1021/ct500490b](https://doi.org/10.1021/ct500490b)

- **IRI**: T. Lu, Q. Chen, _Chemistry-Methods_ 2021, **1**, 231. DOI: [10.1002/cmtd.202100007](https://doi.org/10.1002/cmtd.202100007)

---
