# POD-KNC: Reduced-Order Flow Field Prediction Based on POD and K-Nearest Neighbor Cases

## Overview

This repository contains the implementation of the **POD-KNC (Proper Orthogonal Decomposition – K-Nearest Neighbor Cases)** method proposed for rapid flow field prediction in slurry electrolysis tanks.

The framework combines:

* **Proper Orthogonal Decomposition (POD)** for dimensionality reduction of high-dimensional CFD flow field data;
* **Matrix-analysis-based weighting strategy** for quantifying the influence of operating parameters on POD modal coefficients;
* **K-Nearest Neighbor Cases (KNC)** and inverse-distance interpolation for modal coefficient prediction.

Compared with conventional POD-BPNN surrogate models, POD-KNC avoids neural network training and provides improved computational efficiency while maintaining high prediction accuracy under limited-data conditions.

---

## Methodology

The workflow consists of the following steps:

1. Generate CFD datasets using orthogonal experimental design.
2. Construct the snapshot matrix from velocity field data.
3. Perform POD to extract:

   * Mean flow field
   * POD basis functions
   * Modal coefficients
4. Calculate parameter weights for each POD mode using matrix analysis.
5. Identify nearest neighboring operating conditions using weighted distances.
6. Predict modal coefficients via inverse-distance-weighted interpolation.
7. Reconstruct the target flow field using POD modes and predicted coefficients.

---

## Repository Structure

```text
.
├── POD-KNC.ipynb              # Main notebook
├── or45.csv                   # Orthogonal experimental design table
├── or45_velocity.csv          # CFD velocity field dataset
├── Weights_An45.csv           # Modal coefficient and weighting data
├── An45.csv                   # POD modal coefficients
└── README.md
```

---

## Requirements

The code was developed using Python 3.x.

Required packages:

```bash
numpy
pandas
matplotlib
scikit-learn
```

Install dependencies:

```bash
pip install numpy pandas matplotlib scikit-learn
```

---

## Usage

### 1. Load CFD dataset

```python
or45 = pd.read_csv('or45_velocity.csv', index_col=0)
```

### 2. Perform POD decomposition

```python
U0, An, PhiU, Ds = POD_SNAP(or45.T.values)
```

Outputs:

* `U0` : mean flow field
* `An` : POD modal coefficients
* `PhiU` : POD basis vectors
* `Ds` : eigenvalues

### 3. Calculate POD mode weights

```python
p_w = Or45_matrix_analysis(...)
```

### 4. Search nearest neighboring cases

```python
k_path_idx, weights = or45_find_k_neighbor_case(
    target_parameter,
    mode_weight,
    k
)
```

### 5. Reconstruct target flow field

The predicted modal coefficients are combined with POD basis vectors to reconstruct the velocity field.

---

## Example

Target operating condition:

```python
target_p = [105, 9, 85, 2500]
```

where:

| Parameter | Description               |
| --------- | ------------------------- |
| N         | Impeller rotational speed |
| X         | Solid volume fraction     |
| d         | Particle diameter         |
| ρ         | Particle density          |

The algorithm identifies the nearest neighboring operating conditions and reconstructs the corresponding flow field.

---

## Related Publication

If you use this code in your research, please cite:

```bibtex
@article{wang2025podknc,
  title={Efficient flow field prediction in slurry electrolysis tanks via proper orthogonal decomposition with matrix weighting},
  author={Wang, Wei and Li, Zhenhao and Zhang, Haoran and Miao, Pengyu and Song, Jian and Jiang, Ziyu and Lu, Tingting and Zhao, Hongliang and Liu, Fengqin},
  journal={},
  year={2025}
}
```

(Please update the journal information after publication.)

---

## License

This project is released under the MIT License.

---

## Contact

Wei Wang

State Key Laboratory of Advanced Metallurgy

University of Science and Technology Beijing

Email: [zhaohl@ustb.edu.cn](mailto:zhaohl@ustb.edu.cn)
