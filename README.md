# EyeQ

EyeQ seeks to explore the current capabilities and future potential of quantum computing in image segmentation. Our goal in the 2023 NQN Hackathon was to apply a quantum algorithm to solve a real-world problem in sub-classical time complexity. Prior image segmentation algorithms have traditionally been performed with the minimum-cut algorithm. However, [critics of this approach](https://youtu.be/2IVAznQwdS4) have pointed out issues regarding partition precision -- where some resulting cuts may be trivially small. A newer technique for image segmentation is performed via a [Max-Cut](https://en.wikipedia.org/wiki/Maximum_cut) algorithm, which is an NP-Hard problem. EyeQ seeks to achieve a quantum speedup on this NP-Hard problem.

Image segmentation has a wide array of important applications. For example, in assessing drought conditions, an image of a lake can be segemented into water and land components, and compared over time.

![alt text](https://gray-kpho-prod.cdn.arcpublishing.com/resizer/_en_WcChMkuC4AFSSDXSZfRCr4I=/1200x675/smart/filters:quality(85)/cloudfront-us-east-1.images.arcpublishing.com/gray/VQIYN3ACPZFWZAEXYNCCGIXKRA.png)

![lake no water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_no_water.webp)
![lake only water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/lakes_only_water.webp)

EyeQ's implementation of image segmentation is limited in resolution; as each pixel is represented by one qubit and Aria 1 has 23 qubits, the largest images segmented were 4x4. Nonetheless, this was a successful quantum computing application proof-of-concept, and a valuable learning experience.

## Installation

Download this notebook and upload to a Microsoft Azure Quantum Workspace (see citations section for instructions on setup). Alternatively, you may use a local Azure Python setup if you have one configured.

The notebook has a dependency on `qiskit[optimization]`. If this set of packages is not available in your working environment, run the first code cell or run `pip install qiskit[optimization]` in your environment. Restart your notebook's kernel if necessary.

## Usage

Once `qiskit[optimization]` packages are installed, you are ready to run the notebook. At this point, you may run all cells and use an IonQ quantum computer to segment an image!

At the top of the notebook is a cell with some optional configuration options. The defaults there already work. You may replace the image URL with one you supply and also change the segmentation resolution.

## Explanation

<img width="1512" alt="Screenshot 2023-01-21 at 7 49 47 PM" src="https://user-images.githubusercontent.com/16888236/213899718-b245f7c7-a49a-4823-9ec1-003cdfd1117c.png">

**Preprocessing:** Images are condensed to the desired pixel dimensions—limited principally by the number of available qubits—using bilinear image resizing. The image is then converted into an undirected, weighted, and fully connected graph. Each pixel is mapped to a vertex and its edge weight to each other vertex is equal to their difference in color. (Sum of the absolute value of differences in RGB channels).

**Quantum Algorithm:** The graph is converted to an Ising Hamiltonian which a Variational Quantum Eigensolver (VQE) then solves for the maximum-cut using a qubit for each vertex in the graph. This maximum-cut splits the vertices into two sets that have the greatest edge weights between the sets.

**Postprocessing:** Based on the set assignments in the output, the pixels of the downsized image are grouped and the photo is split into two sets of pixels.


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
