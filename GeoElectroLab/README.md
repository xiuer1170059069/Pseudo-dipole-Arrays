# GeoElectroLab

GeoElectroLab is the 2D DC resistivity inversion software used in this study: *"A Comparative Study of Pseudo-dipole Arrays and Standard Arrays Based on The High-density DC Resistivity Method"*.

It supports inversion of high-density DC resistivity data acquired with the pseudo-dipole array as well as standard arrays (Wenner α/β/γ, dipole-dipole, Schlumberger, etc.), including data transformation between arrays, borehole-to-surface (cross-borehole) surveys, IP inversion, and time-lapse inversion.

## Contents

| Folder | Description |
|---|---|
| `GeoElectroLab/` | Software package (executable, required runtime libraries, and offline help) |
| `TestData/` | Example datasets in various formats (`.dat` ABEM/RES2DINV format, `.dcs`, `.ert`) |

## Usage

1. Extract the package to a local folder.
2. Run `GeoElectroLab.exe` (Windows, 64-bit).
3. Open one of the example files under `../TestData/` to get started.
4. For details, see the offline manual `GeoElectroLab/res/help.chm`.

## Notes

- Large binaries (`*.exe`, `*.dll`, `*.pdb`) are stored with [Git LFS](https://git-lfs.com/). Use `git lfs pull` after cloning, or download the ZIP archive from GitHub directly.
- This is a beta release of GeoElectroLab.
