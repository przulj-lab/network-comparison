# Graphlet-Based Network Comparison Distances

Python tools for computing network comparison distances based on graphlet analysis.

## Authors

- O. N. Yaveroglu
- N. Malod-Dognin
- D. Davis
- Z. Levnajic
- V. Janjic
- R. Karapandza
- A. Stojmirovic
- N. Przulj

**Corresponding Author:** Prof. Natasa Przulj  
📧 natasa.przulj@mbzuai.ac.ae

## Overview

This repository provides Python scripts for computing various network distance measures presented in the paper "Revealing the Hidden Language of Complex Networks". The tools enable quantitative comparison of network structures using graphlet-based methods.

**Paper:** "Revealing the Hidden Language of Complex Networks"

## Network Distance Measures

This toolkit computes eight alignment-free network distance measures:

1. **GCD (Graphlet Correlation Distance)** - Measures differences in graphlet correlation patterns
   - `gcd11` - Non-redundant 2-to-4 node graphlet orbits
   - `gcd15` - All 2-to-4 node graphlet orbits
   - `gcd58` - Non-redundant 2-to-5 node graphlet orbits
   - `gcd73` - All 2-to-5 node graphlet orbits

2. **RGF (Relative Graphlet Frequency Distance)** - Compares relative frequencies of graphlets

3. **GDDA (Graphlet Degree Distribution Agreement)** - Evaluates agreement in graphlet degree distributions
   - `gdda` - Both Arithmetic and Geometric mean variants

4. **Degree Distribution Distance** - Compares node degree distributions

5. **Average Degree Distance** - Measures differences in average network degree

6. **Average Clustering Coefficient Distance** - Compares clustering patterns

7. **Diameter Distance** - Compares network diameters

8. **Spectral Distance** - Uses eigenvalues of the Laplacian representation

## Resources

### Tools Included

- **[Computing GDV Signatures](https://github.com/przulj-lab/network-comparison/blob/main/count.py)** - Script for computing graphlet degree vector signatures
- **[Network Comparison Script](https://github.com/przulj-lab/network-comparison/blob/main/network_comparison.py)** - Main script for computing network distances

*Note: If download links don't work when clicked, copy and paste the URLs directly into your browser's address bar.*

## Requirements

- **Python** (2.7 or 3.x)
- Networks in LEDA `.gw` format
- Multi-threading support for parallel processing

## Installation

```bash
git clone https://github.com/przulj-lab/graphlet-distances.git
cd graphlet-distances
```

## Usage

### Step 1: Convert Networks to LEDA Format

All networks must be in LEDA (`.gw`) format before processing.

### Step 2: Compute Graphlet Degree Vector Signatures

For each network, compute the GDV signatures using the ORCA method:

```bash
python count.py my_network.gw
```

This script:
- Formats the LEDA file for ORCA processing
- Computes graphlet signatures using ORCA
- Outputs `my_network.ndump2` signature file in the same folder

**Output:** `<network_name>.ndump2` file containing graphlet degree vectors for all nodes

### Step 3: Compute Network Distances

With all networks and their signature files in the same folder, compute distances:

```bash
python network_comparison.py network_folder distance_type process_count
```

**Parameters:**
- `network_folder` - Directory containing all `.gw` network files and their `.ndump2` signature files
  - **Important:** `.gw` and `.ndump2` filenames must match exactly (e.g., `network1.gw` and `network1.ndump2`)
- `distance_type` - Distance measure to compute (see options below)
- `process_count` - Number of parallel processes (≥1)

## Distance Type Options

Use the following values for `distance_type` parameter:

| Distance Type | Description |
|---------------|-------------|
| `rgf` | Relative Graphlet Frequency distance |
| `gdda` | Graphlet Degree Distribution Agreement (Arithmetic & Geometric) |
| `degree` | Degree distribution & Average degree distances |
| `clustering` | Average Clustering Coefficient distance |
| `diameter` | Network Diameter distance |
| `gcd11` | Graphlet Correlation Distance (non-redundant 2-4 node orbits) |
| `gcd15` | Graphlet Correlation Distance (all 2-4 node orbits) |
| `gcd58` | Graphlet Correlation Distance (non-redundant 2-5 node orbits) |
| `gcd73` | Graphlet Correlation Distance (all 2-5 node orbits) |
| `spectral` | Spectral distance using Laplacian eigenvalues |

## Example Workflow

1. **Prepare networks in LEDA format**:
   ```bash
   # Place all .gw files in a directory, e.g., networks/
   networks/
   ├── network1.gw
   ├── network2.gw
   └── network3.gw
   ```

2. **Compute GDV signatures for all networks**:
   ```bash
   python count.py networks/network1.gw
   python count.py networks/network2.gw
   python count.py networks/network3.gw
   ```
   
   This creates:
   ```bash
   networks/
   ├── network1.gw
   ├── network1.ndump2    # Generated signature
   ├── network2.gw
   ├── network2.ndump2    # Generated signature
   ├── network3.gw
   └── network3.ndump2    # Generated signature
   ```

3. **Compute GCD-11 distance using 4 processes**:
   ```bash
   python network_comparison.py networks/ gcd11 4
   ```

4. **Compute multiple distance measures**:
   ```bash
   # RGF distance
   python network_comparison.py networks/ rgf 4
   
   # Degree-based distances
   python network_comparison.py networks/ degree 4
   
   # Spectral distance
   python network_comparison.py networks/ spectral 4
   ```

## Output

The script produces **pairwise distance matrices in CSV format** showing distances between all networks in the specified folder. 

The distances are automatically computed for all pairs of networks found in the directory.

Results can be used for:
- Network clustering and classification
- Evolutionary analysis
- Structural comparison
- Network model validation
- Phylogenetic reconstruction

## File Structure

```
.
├── count.py                     # GDV signature computation using ORCA
├── network_comparison.py        # Distance calculation script
├── networks/                    # Directory for network files
│   ├── network1.gw             # Network in LEDA format
│   ├── network1.ndump2         # Generated signature file
│   ├── network2.gw
│   ├── network2.ndump2
│   ├── network3.gw
│   └── network3.ndump2
└── README.md
```

**Important:** The `.gw` and `.ndump2` filenames must match exactly for each network.

## Performance Notes

- Computing GDV signatures can be time-intensive for large networks
- Parallel processing significantly speeds up distance calculations
- Adjust the number of threads based on your system's capabilities

## Related Tools

- **[GRAAL](https://github.com/przulj-lab/GRAAL/)** - Network alignment tool
- **[GraphCrunch](https://github.com/przulj-lab/GraphCrunch/)** - Network analysis tool

## Troubleshooting

### Python Version Issues
If you encounter compatibility issues, try:
```bash
python3 count.py my_network.gw
python3 networkDistance.py network_folder gcd11 4
```

### Memory Issues for Large Networks
For very large networks, consider:
- Using GCD-11 instead of GCD-73
- Reducing the number of parallel threads
- Processing networks in smaller batches

## Implementation Details

**Implemented by:** Omer Nebil Yaveroglu

**Development Timeline:**
- First implementation: June 1, 2012
- Revision 1: August 10, 2012
- Revision 2: August 23, 2012
- Parallelized: October 9, 2012
- Clean Version: May 6, 2014
- ORCA integration (`count.py`): November 5, 2013

## How It Works

The script processes networks through the following steps:

1. **Network Discovery**: Automatically identifies all `.gw` and `.ndump2` files in the specified folder using Python's `os.walk()` function

2. **Signature Computation**: The `count.py` script:
   - Formats LEDA files for ORCA compatibility
   - Computes graphlet degree vectors using ORCA
   - Outputs signatures in `.ndump2` format

3. **Property Calculation**: Depending on the distance measure, computes relevant network properties:
   - Degree distributions
   - Clustering coefficients
   - Network diameter
   - Laplacian eigenvalues (for spectral distance)

4. **Distance Computation**: Calculates log-scaled signatures (log(gd + 1)) and computes pairwise distances in parallel

5. **Output Generation**: Produces CSV-formatted distance matrices

## Contact

For questions or issues, please contact Prof. Natasa Przulj at natasa.przulj@mbzuai.ac.ae
