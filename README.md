<div align="center">

<img src="images/header02.jpg" width="100%">

# Graphlet-Based Network Comparison Distances

**Supplementary information for "Revealing the Hidden Language of Complex Networks"**

**O. N. Yaveroğlu, N. Malod-Dognin, D. Davis, Z. Levnajić, V. Janjić, R. Karapandža, A. Stojmirović and N. Pržulj**

Corresponding author: Prof. N. Pržulj, natasa.przulj@mbzuai.ac.ae

</div>

These are the Python scripts we used to compute the network distances presented in our paper, including the Graphlet Correlation Distance (GCD), the Relative Graphlet Frequency Distance (RGF) and the Graphlet Degree Distribution Agreement (GDDA).

## Scripts

- [Computing GDV signatures](count)
- [Network comparison script](network_comparison.py)

## Usage

1. All networks must be in LEDA (`.gw`) format.

2. Compute the graphlet degree vector (GDV) signatures of each network:
   ```bash
   python count.py my_network.gw
   ```
   This writes `my_network.ndump2` in the same folder.

3. Put all networks and their signature files in the same folder, then compute the distances, e.g. GCD-11:
   ```bash
   python network_comparison.py network_folder gcd11 n
   ```
   where `n` is the number of parallel processes. The result is a matrix of pairwise distances between all networks in the folder.

## Available distances

| Option | Distance |
|---|---|
| `gcd11`, `gcd15`, `gcd58`, `gcd73` | Graphlet Correlation Distance (non-redundant or all orbits, 2–4 or 2–5 node graphlets) |
| `rgf` | Relative Graphlet Frequency distance |
| `gdda` | Graphlet Degree Distribution Agreement (arithmetic and geometric) |
| `degree` | Degree distribution and average degree |
| `clustering` | Average clustering coefficient |
| `diameter` | Network diameter |
| `spectral` | Spectral distance (Laplacian eigenvalues) |

## Citation

Ö. N. Yaveroğlu, N. Malod-Dognin, D. Davis, Z. Levnajić, V. Janjić, R. Karapandža, A. Stojmirović and N. Pržulj. Revealing the hidden language of complex networks. *Scientific Reports* 4, 4547 (2014). https://doi.org/10.1038/srep04547
