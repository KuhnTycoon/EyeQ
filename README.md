# EyeQ

Our goal in the 2023 NQN Hackathon was apply a quantum algorithm to solve a real-world problem in sub-classical time complexity. We choose image segmentation as our problem, which can be classically solved using [Max Cut](https://en.wikipedia.org/wiki/Maximum_cut), a NP-Hard algorithm. 

Image segmentation has a wide array of important applications. For example, if we want to assess drought conditions, we might take an image of a lake and segement it into water and non-water components, and compare the size of the water component to that of previous images of the same lake. 
![alt text](https://gray-kpho-prod.cdn.arcpublishing.com/resizer/_en_WcChMkuC4AFSSDXSZfRCr4I=/1200x675/smart/filters:quality(85)/cloudfront-us-east-1.images.arcpublishing.com/gray/VQIYN3ACPZFWZAEXYNCCGIXKRA.png)

![lake no water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_no_water.webp)
![lake only water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_only_water.webp)

Our implementation of image segmentation is limited in resolution; since each pixel is represented by one qubit, and we can use up to 23 qubits, the largest images we split are 4-by-4. Nonetheless, this was a successful proof-of-concept for applying quantum computing, and a valuable learning experience.

Include photos of lakes, whales, farmers. Original photo, pixeled photo, split pixeled photo, and segmented original photo. Explain as qubits increase resolution will increase as well.

## Installation
Run this notebook
Use Azure Quantum

## Usage
Explain how to run the notebook

## Explanation
**Preprocessing:**
Images are condensed to the desired pixel dimensions—limited principally by the number of available qubits—using bilinear image resizing. The image is then converted into an undirected, weighted, and fully connected graph. Each pixel is mapped to a vertex and its edge weight to each other vertex is equal to their difference in color. (Sum of the absolute value of differences in RGB channels).

**Quantum Algorithm:**
The graph is converted to an Ising Hamiltonian which a Variational Quantum Eigensolver (VQE) then solves for the maximum-cut using a qubit for each vertex in the graph. This maximum-cut splits the vertices into two sets that have the greatest edge weights between the sets.

**Postprocessing:**
Based on the set assignments in the output, the pixels of the downsized image are grouped and the photo is split into two sets of pixels.

## Scaling
Have demonstration of different sizes (Simulator AND QPU AND brute force)
Histograms and graphs

## Citations

## Acknowledgements
