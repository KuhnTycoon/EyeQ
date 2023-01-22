# EyeQ
EyeQ seeks to explore the current capabilities and future potential of quantum computing in image segmenting. Our goal in the 2023 NQN Hackathon was apply a quantum algorithm to solve a real-world problem in sub-classical time complexity. Image segmentation can be classically solved using [Max-Cut](https://en.wikipedia.org/wiki/Maximum_cut), a NP-Hard algorithm. 

Image segmentation has a wide array of important applications. For example, if we want to assess drought conditions, we might take an image of a lake and segement it into water and non-water components, and compare the size of the water component to that of previous images of the same lake. 
![alt text](https://gray-kpho-prod.cdn.arcpublishing.com/resizer/_en_WcChMkuC4AFSSDXSZfRCr4I=/1200x675/smart/filters:quality(85)/cloudfront-us-east-1.images.arcpublishing.com/gray/VQIYN3ACPZFWZAEXYNCCGIXKRA.png)

![lake no water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_no_water.webp)
![lake only water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_only_water.webp)

Our implementation of image segmentation is limited in resolution; as each pixel is represented by one qubit and Aria 1 has 23 qubits, the largest images we segmented were 4x4. Nonetheless, this was a successful quantum computing application proof-of-concept, and a valuable learning experience.

## Installation
Ensure you have a working Python environment. Download this notebook and upload to a Microsoft Azure Quantum Workspace.

## Usage
Explain how to run the notebook

## Explanation
**Preprocessing:**
Images are condensed to the desired pixel dimensions—limited principally by the number of available qubits—using bilinear image resizing. The image is then converted into an undirected, weighted, and fully connected graph. Each pixel is mapped to a vertex and its edge weight to each other vertex is equal to their difference in color. (Sum of the absolute value of differences in RGB channels).



**Quantum Algorithm:**
The graph is converted to an Ising Hamiltonian which a Variational Quantum Eigensolver (VQE) then solves for the maximum-cut using a qubit for each vertex in the graph. This maximum-cut splits the vertices into two sets that have the greatest edge weights between the sets.

**Postprocessing:**
Based on the set assignments in the output, the pixels of the downsized image are grouped and the photo is split into two sets of pixels.

![graphs](https://github.com/KuhnTycoon/EyeQ/blob/main/whale_diagram-removebg-preview%20(1).png?raw=true)

## Scaling

Have demonstration of different sizes (Simulator AND QPU AND brute force)
Histograms and graphs

## Citations
Guides, tutorials, and other resources referenced during this project:
- [Microsoft Azure Quantum setup guide](https://learn.microsoft.com/en-us/azure/quantum/)
- [Qiskit VQE Notebook](https://qiskit.org/documentation/optimization/tutorials/06_examples_max_cut_and_tsp.html)
- [Reducing from 3-SAT to Max Cut](http://www.cs.cornell.edu/courses/cs4820/2014sp/notes/reduction-maxcut.pdf)
## Acknowledgements
We would like to thank Microsoft, IonQ, and the whole NQN consortium for graciously hosting this hackathon. And a huge, huge thank you to the industy mentors
for their help, patience, and good humor this weekend—this wouldn't have been possible without you!
