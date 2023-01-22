# EyeQ
Write and introduction here TODO
Include photos of lakes, whales, farmers. Original photo, pixeled photo, split pixeled photo, and segmented original photo. Explain as qubits increase resolution will increase as well.

## Installation
Run this notebook
Use Azure Quantum

## Usage
Explain how to run the notebook

## Explanation
**Preprocessing:**
Images are condensed to the desired pixel dimensions using bilinear image resizing. The image is then converted into an undirected, weighted, and fully connected graph. Each pixel is mapped to a vertex and its edge weight to each other vertex is equal to their difference in color. (Sum of the absolute value of differences in RGB channels)

**Quantum Algorithm:**
The graph is converted to an Ising Hamiltonian which a Variational Quantum Eigensolver (VQE) then solves for the maximum-cut using a qubit for each vertex in the graph. This maximum-cut splits the vertices into two sets that have the greatest edge weights between the sets.

**Postprocessing:**
Based on the set assignments in the output, the pixels of the downsized image are grouped and the photo is split into two sets of pixels.

## Scaling
Have demonstration of different sizes (Simulator AND QPU AND brute force)
Histograms and graphs

## Citations

## Acknowledgements
