# EyeQ: Quantum Powered Image Segmentation

| Whale 🐳 | Sunset ☀️ | Butterfly 🦋 |
| - | - | - |
| ![whale](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/whaleslider.gif?raw=true) | ![sunset](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/SLIDINGSUN.gif?raw=true) | ![butterfly](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/BUTTERSLIDER.gif?raw=true) |

*Image segmentation demonstrated by EyeQ.* 👆

EyeQ seeks to explore the current capabilities and future potential of quantum computing in image segmentation. Our goal in the 2023 NQN Hackathon was to apply a quantum algorithm to solve a real-world problem in sub-classical time complexity. Prior image segmentation algorithms have traditionally been performed with the minimum-cut algorithm. However, [critics of this approach](https://youtu.be/2IVAznQwdS4) have pointed out issues regarding partition precision—where some resulting cuts may be trivially small. A newer technique for image segmentation is performed via a [Max-Cut](https://en.wikipedia.org/wiki/Maximum_cut) algorithm, which is an NP-Hard problem. EyeQ seeks to use quantum computing to achieve a speedup on this NP-Hard problem.

Image segmentation has a wide array of important applications in the natural sciences, applied mathematics, and machine learning. For example, researchers might use satellite photos of lakes or bodies of water to quantify drought conditions—to do so, an image can be segmented into water and land components, and compared over time, as shown below:

| Original Image | Water Extracted from Image |
| - | - |
| ![lakes](https://gray-kpho-prod.cdn.arcpublishing.com/resizer/_en_WcChMkuC4AFSSDXSZfRCr4I=/1200x675/smart/filters:quality(85)/cloudfront-us-east-1.images.arcpublishing.com/gray/VQIYN3ACPZFWZAEXYNCCGIXKRA.png) | ![lake only water](https://raw.githubusercontent.com/KuhnTycoon/EyeQ/main/Images/lakes_only_water.webp) |

| Scaled Image | Water Area Extracted by EyeQ |
| - | - |
| ![pixel](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/Lakes%20resized.png?raw=true) | ![cut](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/Lakes%20water%20cut.png?raw=true) |

EyeQ's implementation of image segmentation is limited in resolution; as each pixel is represented by one qubit and Aria 1 has 23 qubits, the largest images segmented were 4x4. Nonetheless, this was a successful quantum computing application proof-of-concept and a valuable learning experience.

## Installation

Download this notebook and upload it to a [Microsoft Azure Quantum Workspace](https://learn.microsoft.com/en-us/azure/quantum/). Alternatively, you may use a local Azure Python setup if you have one configured.

The notebook has a dependency on `qiskit[optimization]` and `wget`. If this set of packages is not available in your working environment, run the 2nd and 3rd code cell (under Imports) or run `pip install qiskit[optimization] wget` in your environment. Restart your notebook's kernel if necessary.

## Usage

At the top of the notebook is a cell with configuration options.

```python
### UPDATE WITH YOUR RESOURCE ID
azure_resource_id = ""

w_pixels = 3
url = 'https://img.freepik.com/premium-vector/simple-killer-whale-pixel-art-style_475147-1552.jpg?w=1380'
backend_target = "ionq.simulator" # "ionq.qpu.aria-1" or "ionq.simulator"
```

The only configuration REQUIRED **is to put your resource URL in `azure_resource_id`.**

The defaults for the other options already work. You may replace the segmentation resolution `w_pixels` (number of pixels along an axis) and the quantum backend `backend_target`. We've provided the URL to a sample image for convenience.

Once the packages are installed, you are ready to run the notebook. At this point, you may run all cells and use an IonQ quantum computer to segment an image!

## Explanation

![workflow](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/workflow.png?raw=true)

**Preprocessing:** Images are condensed to the desired pixel dimensions—limited principally by the number of available qubits—using [bilinear image resizing](https://en.wikipedia.org/wiki/Bilinear_interpolation). The image is then converted into an undirected, weighted, and fully connected graph. Each pixel is mapped to a vertex and its edge weight to each other vertex is equal to their difference in color, calculated as the sum of the absolute value of differences in RGB channels.

**Quantum Algorithm:** The graph is converted to an [Ising](https://en.wikipedia.org/wiki/Ising_model) [Hamiltonian](https://en.wikipedia.org/wiki/Hamiltonian_(quantum_mechanics)) which a Variational Quantum Eigensolver ([VQE](https://en.wikipedia.org/wiki/Variational_quantum_eigensolver)) then solves for the maximum-cut using a qubit for each vertex in the graph. This maximum-cut splits the vertices into two sets that have the greatest edge weights between the sets.

**Postprocessing:** Based on the set assignments in the output, the pixels of the downsized image are grouped and the photo is split into two sets of pixels.

## Results & Scaling

Here is EyeQ segmenting the image into 4x4, 3x3, and 2x2 segmentation resolutions. With more qubits, EyeQ can scale to higher segmentation resolutions.

Original Image:

<img src="https://img.freepik.com/premium-vector/simple-killer-whale-pixel-art-style_475147-1552.jpg?w=1380" width=300>

### Simulator results

8192 shots, 10 iterations

| Resolution | Whale | Background | Histogram (probability vs cut solution) |
| - | - | - | - |
| **4x4** | ![4x4 whale](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_4x4_Sim_Snip2-.jpeg?raw=true) | ![4x4 background](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_4x4_Sim_Snip1-.png?raw=true) | ![4x4 hist](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/4x4%20selective%20hist.jpeg?raw=true) |
| **3x3** | ![3x3 whale](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_3x3_Sim_Snip2-smaller.png?raw=true) | ![3x3 background](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_3x3_Sim_Snip1-smaller.png?raw=true) | ![3x3 hist](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/3x3%20hist.jpeg?raw=true) |
| **2x2** | ![2x2 whale](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_2x2_Sim_Snip2.jpeg?raw=true) | ![2x2 other](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/NQN_2x2_Sim_Snip1.jpeg?raw=true) | ![2x2 hist](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/2x2%20hist.jpeg?raw=true) |

Notes and analysis:

- If the resized image is fairly symmetrical, the inverses of the solutions will often produce results of equal probability. For example, the 2x2 graph has nearly equal representation for the solutions 0100 and 1011. Both produce the same cut, just with segments labeled oppositely.
- 4x4: the 2^16 histogram outputs were filtered to show only the significant cuts to improve readability

### IonQ Aria 1 result

8192 shots, 10 iterations, 3x3 segmentation

| Whale | Background | Histogram |
| - | - | - |
| ![whale](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/3x3%20qpu%20whale.jpeg?raw=true) | ![back](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/3x3%20qpu%20other.jpeg?raw=true) | ![hist](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/3x3%20qpu%20hist.jpeg?raw=true) |

Due to time and cost constraints, EyeQ was restricted to 10 iterations which has effects on accuracy. There is also a lot more noise in the solution output space as compared to the noiseless simulator's computation.

The noise in combination with the lower iteration count contributes to the error in the output. For example, here is the classical solution compared to the QPU's solution:

| Classical max cut | QPU max cut |
| - | - |
| ![classical](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/classical%20graph.png?raw=true) | ![qpu](https://github.com/KuhnTycoon/EyeQ/blob/main/Images/qpu%20graph.jpeg?raw=true) |

While there is some overlap in the QPU's max cut solution compared to the classical solution, they are not 100% in agreement.

## Limitations and Future Considerations

### Color Representation

In this project, we compared pixels by RGB channel values, attempting to quantify how similar or how different in color
any two pixels are. However, there are other ways to digitally represent color, like HSV, that may yield more accurate results. For example,
two colors may be visually very different yet have one or two similar RGB channels, and therefore be grouped; with HSV on the other hand, one could
simply compare the Hue channels.

### Number of Qubits

For this to be a practical method of performing image segmentation—or approach for solving similar graph-theoretic problems—access to more qubits is necessary.
In our approach, each pixel (each node in the graph) corresponds to one qubit, and the number of qubits required therefore scales quadratically with the
dimensions of the input image.

## Citations

Guides, tutorials, and other resources referenced during this project:

- [Microsoft Azure Quantum setup guide](https://learn.microsoft.com/en-us/azure/quantum/)
- [Qiskit VQE Notebook](https://qiskit.org/documentation/optimization/tutorials/06_examples_max_cut_and_tsp.html)
- [Reducing from 3-SAT to Max Cut](http://www.cs.cornell.edu/courses/cs4820/2014sp/notes/reduction-maxcut.pdf)

## Acknowledgements

We would like to thank Microsoft, IonQ, and the whole NQN consortium for graciously hosting this hackathon. And a huge, huge thank you to the industry mentors
for their help, patience, and good humor this weekend—this wouldn't have been possible without you!

### Development team

- William Galvin ([william-galvin](https://github.com/william-galvin))
- Andrew Kuhn ([KuhnTycoon](https://github.com/KuhnTycoon))
- Zeynep Toprakbasti ([ladybuglady](https://github.com/ladybuglady))
- Kenneth Yang ([microBob](https://github.com/microBob))
