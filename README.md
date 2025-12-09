# GeoTaichi

## A High Performance Multiscale and Multiphysics Simulator

GeoTaichi is a high-performance computational framework for geomechanics simulations, built on top of Taichi. It supports multiple simulation methods including:

- **DEM** (Discrete Element Method)
- **MPM** (Material Point Method)
- **DEMPM** (Coupled DEM-MPM Method)

## Installation

```bash
pip install geotaichi
```

## Quick Start

```python
import geotaichi as gi

# Initialize GeoTaichi
gi.init(arch="gpu", default_fp="float64")

# Create a DEM simulation
dem = gi.DEM()

# Create an MPM simulation
mpm = gi.MPM()

# Create a coupled DEMPM simulation
dempm = gi.DEMPM()
```

## Features

- High-performance computation with Taichi
- Support for both CPU and GPU execution
- Flexible material models
- Advanced contact handling
- Multiscale and multiphysics coupling capabilities
- Comprehensive visualization tools

## License

This project is licensed under the GNU General Public License v3.0 - see the LICENSE file for details.

## Authors

- Shi-Yihao, Guo-Ning
- Multiscale Geomechanics Lab, Zhejiang University
