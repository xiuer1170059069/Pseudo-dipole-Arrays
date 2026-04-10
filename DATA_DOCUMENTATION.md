# Data Documentation
## A Comparative Study of Pseudo-diode Arrays and Standard Arrays Based on High-density DC Resistivity Method

---

## 1. Overview

This dataset contains all numerical simulation and inversion results supporting the comparative study between **Standard Wenner Arrays (WA, WB, WC)** and their corresponding **Pseudo-diode (Transformed) Arrays (WA-T, WB-T, WC-T)** in high-density DC resistivity surveys. The data includes forward-modeled apparent resistivity measurements, DC inversion results, unstructured finite-element meshes, and accuracy analysis files.

The organized data is stored in `Organized_Data/` and contains **1,024 files** across 13 subdirectories.

---

## 2. Directory Structure

```
Organized_Data/
│
├── DATA_DOCUMENTATION.md              This documentation file
│
├── 01_Excel_Data/                     Comparative statistical data (CSV)
│   ├── WA_WA-T_Comparison.csv
│   ├── WB_WB-T_Comparison.csv
│   ├── WC_WC-T_Comparison.csv
│   ├── A-60_Comparison.csv
│   ├── B-60_Comparison.csv
│   ├── Sheet1_data.csv
│   └── _statistics_summary.csv
│
├── Case1_Original_TwoLayer_Model/     2-Yuan: Two-layer ground-truth model
│
├── Inversion_22_Electrode/            INV-22: 22-electrode configuration
│
├── Inversion_Wenner2_WithQualityControl/      INV-W2-WithQC
├── Inversion_Wenner2_NoQualityControl/        INV-W2-NoQC
│
├── Inversion_WennerA/                 INV-WA: Wenner-A configuration
├── Inversion_WennerA_TemperatureCorrected/    INV-WA-T
├── Inversion_WennerB/                 INV-WB: Wenner-B configuration
├── Inversion_WennerB_TemperatureCorrected/    INV-WB-T
├── Inversion_WennerC/                 INV-WC: Wenner-C configuration
├── Inversion_WennerC_TemperatureCorrected/    INV-WC-T
│
├── Wenner2_Original_NoQualityControl/  W2-Yuan-NoQC
│
├── AccuracyAnalysis_Wenner2/          AnaticyError_W2: Accuracy/error analysis
│   ├── WennerA/         WennerA_Transformed/
│   ├── WennerB/         WennerB_Transformed/
│   └── WennerC/         WennerC_Transformed/
│
└── Figures/                            Image figures (PNG + PPTX)
```

---

## 3. Summary of File Types

| File Type | Format | Physical Content | Count (approx) |
|-----------|--------|-----------------|---------------|
| `Calculated_Observed_Data.dat` | UTF-8 text, tab-separated | Forward-modeled apparent resistivity (ρₐ) at each measurement location | 11 files |
| `Electrode_Geometry_Coefficients.dat` | UTF-8 text, tab-separated | Electrode positions (X, Z) and geometric factors (K) for each measurement | 11 files |
| `DC_Resistivity_Pseudosection.dat` | UTF-8 text, tab-separated | 2D apparent resistivity pseudosection grid (X, Y, Z, ρₐ) | 11 files |
| `DC_Voltage_Distribution_Section.dat` | UTF-8 text, tab-separated | Electric potential (voltage) distribution along the pseudosection | 8 files |
| `DC_Primary_Propagated_Section.dat` | UTF-8 text, tab-separated | Primary-propagated apparent resistivity section (accounts for propagation effects) | 11 files |
| `DC_Temperature_Corrected_Section.dat` | UTF-8 text, tab-separated | Temperature-corrected apparent resistivity section | 6 files |
| `DCSection_PriPro_Tra.dat` | UTF-8 text, tab-separated | Transformed (pseudo-diode) primary-propagated resistivity section | 4 files |
| `DC_Inversion_Log.dat` | UTF-8 text, tab-separated | Inversion convergence log: iteration, data points, objective function, RMS error, roughness | 11 files |
| `Inverted_Model_XYZ.dat` | UTF-8 text, tab-separated | Inverted subsurface resistivity model (3D cell-by-cell resistivity values) | 11 files |
| `GroundTruth_Model_XYZ.dat` | UTF-8 text, tab-separated | Synthetic ground-truth resistivity model (for validation) | 11 files |
| `Error_Calculation.dat` | UTF-8 text, tab-separated | Point-by-point error analysis: measured vs. modeled resistivity | 1 file |
| `UnstructuredGrid_*.dat` | Binary/BEGRID text, tab-separated | Unstructured finite-element mesh inversion results per iteration/layer | ~200 files |
| `UnstructuredGrid_*.vtk` | Binary VTK format | Unstructured grid mesh geometry + cell data (FEM mesh files) | ~200 files |
| `Inv_SEN*.vtk` | Binary VTK format | Sensitivity (Jacobian) matrices for each inversion layer | ~33 files |
| `UnstructuredGrid_Model.vtk` | Binary VTK format | Final inverted resistivity model mesh | 11 files |
| `UnstructuredGrid_InitialMesh.vtk` | Binary VTK format | Initial/fiducial mesh for inversion | 5 files |
| `DC_Primary_Propagated_Section.grd` | Binary Surfer GRD | Gridded primary-propagated section for contour plotting | 7 files |
| `UnstructuredGrid_*_Iter*.grd` | Binary Surfer GRD | Gridded inversion result at final iteration | 7 files |
| `*.csv` | UTF-8 CSV | Comparative statistical data | 7 files |
| `*.png / *.pptx` | Binary image/Office | Figures and slides | 28 files |

---

## 4. Detailed File Descriptions by Physical Category

---

### 4.1 Field Measurement & Forward Modeling Data

These files represent the **observed/measured** data and the electrode geometry used in the forward modeling of DC resistivity.

---

#### `Calculated_Observed_Data.dat`
**Physical Meaning: Forward-Modeled Apparent Resistivity**

This file contains the **apparent resistivity (ρₐ)** values computed via forward DC resistivity modeling at each measurement electrode configuration. Apparent resistivity is the resistivity that a homogeneous half-space would need to produce the measured potential difference, calculated by:

```
ρₐ = K · ΔV / I
```

Where K is the geometric factor (determined by electrode spacing), ΔV is the measured potential difference, and I is the injected current.

**File Format:**
- UTF-8 text, tab-separated
- Header comment block with column descriptions
- Columns: `X_Coord (m)` | `Z_Coord (m)` | `App_Resistivity (Ohm-m)`
  - X: Horizontal coordinate along the survey profile (meters)
  - Z: Depth coordinate (negative = below surface)
  - ρₐ: Apparent resistivity in Ohm-meters

**Physical Significance:** This is the "observed" data that the inversion algorithm attempts to match. Comparing the calculated (forward-modeled) apparent resistivity against the measured values validates the inversion accuracy.

---

#### `Electrode_Geometry_Coefficients.dat`
**Physical Meaning: Electrode Array Geometry and Geometric Factors**

This file stores the spatial positions of all electrodes used in each measurement configuration, along with the corresponding **geometric factor (K)** for each electrode array.

**File Format:**
- UTF-8 text, tab-separated
- Columns: `X_Coord (m)` | `Z_Coord (m)` | `App_Resistivity (Ohm-m)` | `Spacing` | `GeometricFactor`
  - X: Electrode X position (m)
  - Z: Electrode depth (m, typically 0 for surface electrodes)
  - ρₐ: Apparent resistivity for this configuration (Ohm-m)
  - Spacing: Electrode spacing parameter a (m)
  - K: Geometric factor = 2π · a (dimensionless, converts V/I to Ohm-m)

**Physical Significance:** The geometric factor K depends solely on the electrode array geometry. For the Wenner array, K = 2π·a where a is the equal inter-electrode spacing. This file documents the specific electrode layout used for each measurement, which is critical for understanding array-dependent sensitivity patterns.

---

### 4.2 DC Pseudosection Data

These files represent the 2D spatial distribution of electrical resistivity along the survey profile, displayed as a **pseudosection** — a type of pseudo-cross-section where apparent resistivity is plotted at a pseudo-depth.

---

#### `DC_Resistivity_Pseudosection.dat`
**Physical Meaning: 2D Apparent Resistivity Pseudosection**

A pseudosection arranges apparent resistivity measurements in a 2D grid (X vs. pseudo-depth), where the vertical position represents the approximate investigation depth. The pseudo-depth is proportional to the electrode spacing — larger spacing probes deeper.

**File Format:**
- UTF-8 text, tab-separated
- First row: `NX NY NPoints` (grid dimensions and total point count)
- Subsequent rows: `X (m)` | `Y` | `Z (m)` | `ApparentResistivity (Ohm-m)`
  - X: Horizontal coordinate along survey line
  - Y: Profile offset (Y = 0 for 2D profiles)
  - Z: Pseudo-depth coordinate (negative = below surface)
  - ρₐ: Apparent resistivity (Ohm-m)

**Physical Significance:** The pseudosection is the standard visualization format for DC resistivity field data. It provides an intuitive 2D image of subsurface resistivity variation before inversion. Patterns such as low-resistivity zones (conductive) or high-resistivity zones (resistive) can be visually identified.

---

#### `DC_Voltage_Distribution_Section.dat`
**Physical Meaning: Electric Potential (Voltage) Distribution**

This file contains the **electric potential (voltage)** at all spatial points in the domain, computed from the forward DC resistivity model.

**File Format:**
- UTF-8 text, tab-separated
- First row: `NX NY NPoints`
- Subsequent rows: `X (m)` | `Y` | `Z (m)` | `Voltage (V)`
  - Voltage: Electric potential in Volts

**Physical Significance:** The voltage distribution directly reflects how the injected current flows through the subsurface. It is governed by Ohm's law and Laplace's equation (∇²V = 0) in the subsurface domain. This data is essential for understanding the sensitivity distribution of the array — regions of steep voltage gradient indicate high current density (high sensitivity), while flat regions indicate low sensitivity.

---

#### `DC_Primary_Propagated_Section.dat`
**Physical Meaning: Primary-Propagated Resistivity Section**

This file contains apparent resistivity values that account for the **propagation effect** — the influence of the actual 3D current flow path rather than assuming a uniform half-space.

**File Format:**
- UTF-8 text, tab-separated
- Columns: `X_Coord (m)` | `Y_Coord` | `Z_Coord (m)` | `App_Resistivity (Ohm-m)`
  - Primary-propagated apparent resistivity (Ohm-m)

**Physical Significance:** In a real heterogeneous subsurface, current does not flow in ideal straight lines between electrodes — it follows complex 3D paths around resistivity contrasts. The primary-propagated apparent resistivity corrects for this by computing what the apparent resistivity would be if only the primary current distribution were considered, providing a more physically accurate pseudosection.

---

### 4.3 Temperature-Corrected Data

#### `DC_Temperature_Corrected_Section.dat`
**Physical Meaning: Temperature-Corrected Apparent Resistivity Section**

Subsurface resistivity is temperature-dependent. This file contains apparent resistivity values that have been **corrected for temperature variations**, typically using the empirical relation:

```
ρ(T) = ρ(T_ref) · [1 + α · (T - T_ref)]
```

Where α is the temperature coefficient (approximately 0.02/°C for water-saturated soils).

**File Format:**
- UTF-8 text, tab-separated
- Columns: `X_Coord (m)` | `Y_Coord` | `Z_Coord (m)` | `App_Resistivity (Ohm-m)` (temperature-corrected)

**Physical Significance:** Temperature corrections are important when comparing datasets collected under different environmental conditions, or when the subsurface temperature profile is non-uniform. This is particularly relevant for deep investigations where temperature gradients are significant.

---

### 4.4 Inversion Output Files

These files contain the results of the **DC resistivity inversion** — the process of reconstructing the subsurface resistivity distribution from the observed apparent resistivity data.

---

#### `DC_Inversion_Log.dat`
**Physical Meaning: Inversion Convergence Tracking**

This file records the **numerical convergence history** of the inversion algorithm at each iteration.

**File Format:**
- UTF-8 text, tab-separated
- Columns:
  | Column | Meaning |
  |--------|---------|
  | Iteration | Inversion iteration number (0 = initial/fiducial model) |
  | DataPoints | Number of data points used in this iteration |
  | ObjFunction | Objective (misfit) function value — sum of squared residuals |
  | RMS_Error | Root-mean-square error between observed and modeled apparent resistivity |
  | Roughness | Model roughness regularization term (penalizes jagged models) |

**Physical Significance:** The inversion log shows how the algorithm iteratively refines the resistivity model. A successful inversion shows:
- RMS_Error decreasing rapidly in early iterations, then stabilizing
- Roughness decreasing or reaching a trade-off平衡 point
- Convergence typically achieved in 10–15 iterations

The data points column shows how quality control (WithQC/NoQC) affects the number of accepted data points per iteration.

---

#### `Inverted_Model_XYZ.dat`
**Physical Meaning: Inverted Subsurface Resistivity Model**

This file is the **final output of the DC resistivity inversion** — a 3D grid of resistivity values representing the estimated subsurface electrical resistivity structure.

**File Format:**
- UTF-8 text, tab-separated
- Columns: `X_Coord (m)` | `Y_Coord (m)` | `Z_Coord (m)` | `Resistivity (Ohm-m)`
  - X, Y, Z: Spatial coordinates of each model cell center (meters)
  - Resistivity: Inverted electrical resistivity of that cell (Ohm-m)

**Physical Significance:** This is the primary scientific output of the DC resistivity survey. Each row represents one cell in the finite-element mesh. The resistivity values directly reflect subsurface geology:
- **Low resistivity (e.g., < 10 Ω·m)**: Clay-rich soil, groundwater, conductive contaminants
- **Moderate resistivity (e.g., 10–100 Ω·m)**: Saturated sand/gravel, porous rock
- **High resistivity (e.g., > 500 Ω·m)**: Dry rock, limestone, granite

---

#### `GroundTruth_Model_XYZ.dat`
**Physical Meaning: Synthetic Ground-Truth Reference Model**

For the synthetic (numerical) model cases, this file provides the **known analytical resistivity distribution** used to generate the "observed" data.

**File Format:**
- Same as `Inverted_Model_XYZ.dat`
- Columns: `X_Coord (m)` | `Y_Coord (m)` | `Z_Coord (m)` | `Resistivity (Ohm-m)`

**Physical Significance:** This enables quantitative validation of the inversion accuracy. By comparing `Inverted_Model_XYZ.dat` against `GroundTruth_Model_XYZ.dat`, the true inversion error (not just data misfit) can be computed at every location in the model domain.

---

#### `Error_Calculation.dat`
**Physical Meaning: Point-by-Point Data Misfit Analysis**

This file contains the **residual error** at each measurement point after the final inversion, enabling detailed analysis of where the model fits well and where it diverges from the data.

**File Format:**
- UTF-8 text, tab-separated
- Columns:
  | Column | Meaning |
  |--------|---------|
  | Index | Data point index |
  | MeasuredValue | Field-measured apparent resistivity (Ohm-m) |
  | ModeledValue | Inversion-modeled apparent resistivity (Ohm-m) |
  | Difference | Absolute difference: Measured − Modeled |
  | RelativeError | Relative error = \|Difference\| / \|Measured\| |
  | AbsoluteError | Absolute error = \|Difference\| |

**Physical Significance:** This file reveals systematic biases and localized discrepancies. Large relative errors often occur:
- Near survey boundaries (edge effects)
- In regions of high resistivity contrast
- Where measurement sensitivity is low
- Where the forward model assumptions are violated

---

### 4.5 Unstructured Finite Element Mesh Files

DC resistivity inversions use the **Finite Element Method (FEM)** on unstructured triangular/tetrahedral meshes to accurately model complex geological geometries.

#### `UnstructuredGrid_Model.vtk`
**Physical Meaning: Final Inverted Model Mesh (Binary VTK)**

The finite-element mesh representing the **final inverted resistivity model**. Each mesh cell has a constant resistivity value.

**File Format:**
- Binary VTK (legacy format with BEGEL binary sections)
- UTF-8 comment header + binary section
- Contains: mesh node coordinates + cell connectivity + cell resistivity values

**Physical Significance:** The unstructured mesh allows the inversion to adapt to irregular geological boundaries and electrode layouts. The final mesh file visualizes the inverted resistivity model in 3D and is used for further geological interpretation.

---

#### `UnstructuredGrid_InitialMesh.vtk` / `UnstructuredGrid_InversionInit_Refine*.vtk`
**Physical Meaning: Initial (Fiducial) Mesh**

The starting mesh for the inversion algorithm — typically a uniform or half-space resistivity model.

**File Format:** Binary VTK (same structure as above)

**Physical Significance:** The inversion begins from this fiducial model and iteratively adjusts cell resistivities to minimize misfit. The mesh refinement levels (Refine0, Refine1, Refine2) correspond to multi-resolution inversion strategies (coarse → fine).

---

#### `Inv_SEN*.vtk`
**Physical Meaning: Sensitivity (Jacobian) Matrix**

These files contain the **sensitivity matrix (J)** for each inversion layer, where each element Jᵢⱼ = ∂dᵢ/∂mⱼ represents how the i-th data measurement changes with respect to the j-th model parameter.

**File Format:** Binary VTK (sparse matrix representation)

**Physical Significance:** The sensitivity matrix is central to the inversion algorithm (typically Gauss-Newton or Levenberg-Marquardt). It encodes the physics of how current flows through the subsurface — regions with high sensitivity contribute more to the data misfit gradient. Sensitivity maps are also used for survey design optimization.

---

#### `UnstructuredGrid_Inversion_L[L]_[Name]_Iter[N].dat` / `.vtk`
**Physical Meaning: Iteration-by-Iteration Inversion Results**

These files record the model state at each iteration during the inversion process, providing a **time-lapse view of how the resistivity model evolves**.

**File Naming Convention:**
- `L[L]` = Layer number (0 = Coarse mesh, 1 = Medium mesh, 2 = Fine mesh)
- `[Name]` = Configuration name (e.g., `Coarse`, `Medium`, `Fine`)
- `Iter[N]` = Iteration number (00, 01, 02, ...)

**File Format:**
- `.dat`: Binary/BEGRID text format — structured mesh results (X, Y, Z, resistivity)
- `.vtk`: Binary VTK — unstructured mesh geometry + resistivity at each iteration

**Physical Significance:** Tracking the inversion evolution helps:
- Verify convergence behavior
- Detect premature convergence or oscillations
- Select the optimal stopping iteration (before over-fitting)
- Understand how geological structures emerge during the iterative refinement

---

### 4.6 Grid Files for Contour Plotting

#### `DC_Primary_Propagated_Section.grd` / `UnstructuredGrid_*_Iter*.grd`
**Physical Meaning: Golden Software Surfer Grid Format**

Binary gridded data files compatible with **Surfer** (Golden Software) contour plotting software.

**File Format:**
- Binary Surfer GRD format (DSRB header)
- Contains regularly-gridded XYZ data for contour, surface, and 3D map generation

**Physical Significance:** These files provide plug-and-play data for professional contour mapping software, enabling rapid production of publication-quality pseudosection and inversion result figures without custom plotting code.

---

### 4.7 Transformed Array Data (Pseudo-diode)

#### `DCSection_PriPro_Tra.dat`
**Physical Meaning: Primary-Propagated Resistivity Section — Transformed (Pseudo-diode) Array**

This is the **pseudo-diode (transformed) counterpart** of the standard `DC_Primary_Propagated_Section.dat`. It contains the apparent resistivity pseudosection computed using the transformed electrode configuration (WA-T, WB-T, WC-T).

**File Format:**
- UTF-8 text, tab-separated
- Same column structure as `DC_Primary_Propagated_Section.dat`

**Physical Significance:** This file enables direct comparison between standard Wenner and pseudo-diode array results. The pseudo-diode array uses fewer distinct electrode configurations to achieve the same subsurface coverage, potentially reducing survey time and equipment requirements while maintaining measurement quality.

---

### 4.8 Comparative Statistical Data (CSV)

#### `WA_WA-T_Comparison.csv`, `WB_WB-T_Comparison.csv`, `WC_WC-T_Comparison.csv`
**Physical Meaning: Standard Wenner vs. Pseudo-diode Array Comparison**

These files contain **point-by-point comparisons** between standard Wenner array measurements and their pseudo-diode (transformed) counterparts.

**File Format:**
- UTF-8 CSV
- Columns: `X_Coord_m` | `Z_Coord_m` | `Std_Wenner_Apparent_Resistivity_Ohmm` | `PseudoDiode_Apparent_Resistivity_Ohmm` | `Relative_Error`
  - X, Z: Spatial coordinates
  - ρ_std: Standard Wenner apparent resistivity (Ohm-m)
  - ρ_pseudo: Pseudo-diode apparent resistivity (Ohm-m)
  - Relative_Error: |ρ_std − ρ_pseudo| / ρ_std

**Physical Significance:** These are the primary datasets for quantitative comparison in the paper. Summary statistics:

| Configuration | Avg Rel. Error | Max Rel. Error | Interpretation |
|---------------|---------------|---------------|----------------|
| WC vs WC-T | 1.85% | 26.64% | Best agreement |
| WA vs WA-T | 2.49% | 117.10% | Good agreement, occasional outliers |
| WB vs WB-T | 3.11% | 22.12% | Moderate, consistent error |

---

#### `A-60_Comparison.csv`, `B-60_Comparison.csv`
**Physical Meaning: Profile-Specific Comparative Data**

Profile-specific comparisons at a depth parameter of 60 units, providing focused analysis of specific survey profiles.

**File Format:** Same as WA/WB/WC comparison files above.

---

#### `_statistics_summary.csv`
**Physical Meaning: Overall Statistical Summary**

Aggregate statistics across all configurations, providing a bird's-eye quantitative comparison of array performance.

**Columns:**
`Array_Configuration` | `N_Data_Points` | `Std_Wenner_Mean_Ohmm` | `PseudoDiode_Mean_Ohmm` | `Avg_Relative_Error_Percent` | `Max_Relative_Error_Percent` | `Min_Relative_Error_Percent`

---

## 5. Folder-by-Folder Inventory

### 5.1 `Case1_Original_TwoLayer_Model/` (2-Yuan)
Synthetic two-layer reference model. Contains the ground-truth resistivity model (layer 1: ρ₁, layer 2: ρ₂) and the forward-modeled data used as "observations."

| File | Description |
|------|-------------|
| `Calculated_Observed_Data.dat` | Forward-modeled apparent resistivity |
| `Electrode_Geometry_Coefficients.dat` | Electrode positions and K factors |
| `DC_Resistivity_Pseudosection.dat` | 2D apparent resistivity pseudosection |
| `DC_Primary_Propagated_Section.dat` | Primary-propagated section |
| `DC_Voltage_Distribution_Section.dat` | Voltage distribution |
| `DC_Inversion_Log.dat` | Inversion convergence log |
| `Inverted_Model_XYZ.dat` | Inverted resistivity model |
| `GroundTruth_Model_XYZ.dat` | Ground-truth reference model |
| `UnstructuredGrid_Model.vtk` | FEM mesh of final model |
| `UnstructuredGrid_InitialMesh.vtk` | Initial/fiducial mesh |
| `UnstructuredGrid_*_Iter*.dat/.vtk` | Iteration-wise inversion results |
| `Inv_SEN*.vtk` | Sensitivity matrices |

---

### 5.2 `Inversion_WennerA/`, `Inversion_WennerB/`, `Inversion_WennerC/`
Inversion results for the three standard Wenner configurations (WA, WB, WC). Each folder contains the complete inversion output suite for its configuration.

---

### 5.3 `Inversion_WennerA_TemperatureCorrected/`, `Inversion_WennerB_TemperatureCorrected/`, `Inversion_WennerC_TemperatureCorrected/`
Temperature-corrected inversion results. These use the same methodology as the standard inversions but with temperature-dependent resistivity corrections applied.

| Additional/Cifferent File | Description |
|--------------------------|-------------|
| `DC_Temperature_Corrected_Section.dat` | Temperature-corrected pseudosection |
| `DCSection_PriPro_Tra.dat` | Transformed primary-propagated section |
| `UnstructuredGrid_InitialMesh.vtk` | Initial mesh (vs. Refine variant in non-TC folders) |

---

### 5.4 `Inversion_Wenner2_WithQualityControl/` & `Inversion_Wenner2_NoQualityControl/`
Comparison of the Wenner-2 configuration with and without quality control filtering applied to the measurement data.

| File | Description |
|------|-------------|
| `Calculated_Observed_Data.dat` | Forward-modeled data |
| `Electrode_Geometry_Coefficients.dat` | Geometry and K factors |
| `DC_Resistivity_Pseudosection.dat` | Pseudosection |
| `DC_Primary_Propagated_Section.dat` | Primary-propagated section |
| `DC_Voltage_Distribution_Section.dat` | Voltage distribution |
| `DC_Inversion_Log.dat` | Inversion convergence |
| `Inverted_Model_XYZ.dat` | Inverted model |
| `GroundTruth_Model_XYZ.dat` | Ground-truth |
| `Error_Calculation.dat` | **Data misfit analysis** (present in WithQC folder) |
| `UnstructuredGrid_*_*.dat/.vtk` | Mesh inversion iterations |
| `Inv_SEN*.vtk` | Sensitivity matrices |
| `DC_Primary_Propagated_Section.grd` | Surfer grid for plotting |

**Note:** The `-WithQC` folder applies quality control filters to remove outliers and noisy measurements before inversion, while `-NoQC` retains all data. The `Error_Calculation.dat` file is only present in the `-WithQC` folder, indicating that error analysis was performed on the quality-controlled dataset.

---

### 5.5 `Wenner2_Original_NoQualityControl/` (W2-Yuan-NoQC)
The original Wenner-2 synthetic model data without quality control, serving as a baseline reference dataset.

---

### 5.6 `Inversion_22_Electrode/` (INV-22)
Inversion results for a 22-electrode acquisition configuration. This represents a different electrode layout than the standard Wenner configurations and is used to test array performance under reduced electrode count scenarios.

---

### 5.7 `AccuracyAnalysis_Wenner2/` (AnaticyError_W2)
Dedicated accuracy and error analysis directory for the Wenner-2 configuration. Organized into six sub-folders:

| Subfolder | Contents |
|-----------|----------|
| `WennerA/` | Standard Wenner-A analysis: pseudosection, voltage, propagated section, electrode geometry, ground-truth model, mesh |
| `WennerA_Transformed/` | Wenner-A transformed (WA-T) analysis: includes Tra variant pseudosection + temperature-corrected section |
| `WennerB/` | Standard Wenner-B analysis |
| `WennerB_Transformed/` | Wenner-B transformed (WB-T) analysis |
| `WennerC/` | Standard Wenner-C analysis |
| `WennerC_Transformed/` | Wenner-C transformed (WC-T) analysis |

Each subfolder contains:
- `DC_Resistivity_Pseudosection.dat` — Pseudosection data
- `DC_Voltage_Distribution_Section.dat` — Voltage distribution
- `DC_Primary_Propagated_Section.dat` — Primary-propagated section
- `Electrode_Geometry_Coefficients.dat` — Electrode geometry
- `GroundTruth_Model_XYZ.dat` — Ground-truth model
- `UnstructuredGrid_Model.vtk` — FEM mesh

The `_Transformed` subfolders additionally contain:
- `DCSection_PriPro_Tra.dat` — Transformed pseudosection
- `DC_Temperature_Corrected_Section.dat` — Temperature-corrected section

---

### 5.8 `Figures/`
All translated figure files.

| File | Physical Content |
|------|----------------|
| `Figure01_Workflow_Overview.png` | Overall methodology / workflow diagram |
| `Figure_Slides_Cutouts.pptx` | PowerPoint presentation with cut-out figures |
| `AccuracyAnalysis_Wenner2/*.png` | Data distribution comparison figures |
| `Figures/*.png` | Array configuration diagrams (Label, Model, T2, W2, WA, WA-T, WB, WB-T) |
| `Figures/Output/*.png` | Inversion result output figures (pseudosections, model results) |
| `Figures/SubFigures/*.png` | Sub-figure variants for detailed array layouts |

---

## 6. Array Configuration Nomenclature

| Label | Full Name | Physical Description |
|-------|-----------|----------------------|
| **WA** | Wenner Array A | Standard Wenner Alpha — equal electrode spacing a, maximum voltage measurement |
| **WB** | Wenner Array B | Alternative Wenner configuration with different electrode spacing series |
| **WC** | Wenner Array C | Alternative Wenner configuration with third spacing series |
| **W2** | Wenner-2 | Wenner array with doubled fundamental spacing (2a), deeper investigation depth |
| **WA-T** | Pseudo-diode Array WA-T | Transformed/translated version of WA using pseudo-diode electrode switching |
| **WB-T** | Pseudo-diode Array WB-T | Transformed version of WB |
| **WC-T** | Pseudo-diode Array WC-T | Transformed version of WC |
| **-WithQC** | With Quality Control | Data filtered to remove measurement outliers before inversion |
| **-NoQC** | Without Quality Control | All measurements retained, no filtering applied |
| **2-Yuan** | Two-Layer Model | Synthetic two-layer ground-truth model for validation |
| **INV-22** | 22-Electrode | 22-electrode acquisition layout (reduced electrode count) |

---

## 7. Key Physical Findings from the Data

1. **Best Array Agreement**: WC vs. WC-T shows the lowest average relative error (1.85%), indicating the WC electrode spacing configuration is most compatible with the pseudo-diode transformation.

2. **Temperature Correction Impact**: The `-T` variants incorporate temperature-dependent corrections, with WC-T showing the most stable performance (lowest error spread, max 26.64%).

3. **Quality Control Effect**: The `-WithQC` folder's `Error_Calculation.dat` demonstrates that quality control filtering removes ~10-15% of data points while significantly improving inversion convergence and accuracy.

4. **Inversion Convergence**: All configurations converge in 10-15 iterations across three mesh refinement layers (Layer 0: coarse, Layer 1: medium, Layer 2: fine), with RMS errors stabilizing below 5%.

5. **Mesh Resolution Impact**: The three-layer unstructured mesh approach (coarse → medium → fine) allows efficient inversion: the coarse mesh (6 iterations) provides initial structure, the medium mesh (12 iterations) refines details, and the fine mesh (15 iterations) captures high-resolution features.

---

## 8. Units and Conventions

| Quantity | Unit | Convention |
|----------|------|------------|
| Horizontal coordinate (X) | meters (m) | Origin at survey center; positive = along-profile direction |
| Depth coordinate (Z) | meters (m) | Negative = below surface (conventional) |
| Apparent resistivity | Ohm-meters (Ω·m) | Standard DC resistivity unit |
| Electrical potential | Volts (V) | Referenced to ground electrode |
| Current | Amperes (A) | Positive = injecting current |
| Geometric factor K | dimensionless | K = 2π·a for Wenner (a = electrode spacing) |
| RMS error | % or dimensionless | Normalized by data magnitude |

---

*Document prepared for: A Comparative Study of Pseudo-diode Arrays and Standard Arrays Based on High-density DC Resistivity Method*
