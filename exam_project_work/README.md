<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<br />
<div align="center">
  <h3 align="center">Exam Project Work &mdash; Obnoxious <em>p</em>-Median Problem (OpMP)</h3>

  <p align="center">
    <strong>Unified Model Formulation, Big-M Constraint Logic, and Gurobi Exact Optimization</strong>
    <br />
    <em>University of L'Aquila (UnivAQ) &bull; Master's Degree in Computer Science (Decision Optimization)</em>
    <br />
    <br />
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/blob/main/exam_project_work/IV_formulation.ipynb"><strong>Open Notebook (IV_formulation.ipynb) »</strong></a>
    <br />
    <br />
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization">Main Repository</a>
    &middot;
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/blob/main/exam_project_work/reference_paper/1-s2.0-S0166218X2030490X-main.pdf">Reference Paper</a>
    &middot;
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/issues">Issues</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#theoretical-background">Theoretical Background</a></li>
        <li><a href="#mathematical-formulation">Mathematical Formulation</a></li>
        <li><a href="#project-structure">Project Structure</a></li>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li>
      <a href="#usage">Usage</a>
      <ul>
        <li><a href="#1-toy-model--graph-construction">1. Toy Model & Graph Construction</a></li>
        <li><a href="#2-gurobi-model-optimization">2. Gurobi Model Optimization</a></li>
        <li><a href="#3-correctness-testing-via-brute-force">3. Correctness Testing via Brute-Force</a></li>
        <li><a href="#4-benchmark-dataset-experiments">4. Benchmark Dataset Experiments</a></li>
      </ul>
    </li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

This directory contains the core implementation for the individual project work submitted for the **Decision Optimization** course in the **Master's Degree in Computer Science** at the **University of L'Aquila (UnivAQ)**.

The project investigates exact optimization methods for the **Obnoxious $p$-Median Problem (OpMP)**, focusing on the unified dispersion formulation from literature, implementing an exact Mixed-Integer Linear Programming (MILP) model in Python with Gurobi, verifying correctness against exhaustive combinatorial search, and benchmarking on recognized literature datasets.

### Theoretical Background

In traditional facility location theory (e.g., $p$-median, $p$-center), facilities provide desirable goods or services, and the goal is to locate them near demand points to minimize overall transportation or access costs.

In contrast, **obnoxious facility location** focuses on undesirable facilities (such as hazardous waste dumps, polluting chemical factories, noisy substations, or incinerators) whose presence degrades the quality of life or safety of nearby communities. The goal of the **Obnoxious $p$-Median Problem (OpMP)** is to select $p$ locations among $m$ candidate facility sites to maximize the sum of distances from each client to its closest open facility, effectively dispersing facilities as far from client populations as possible.

This work specifically implements the **Unified Model for dispersing facilities from clients** (referred to as Formulation IV), derived from:
* **Chang-Chun Lin & Yen-I Chiang (2021)**, *"Alternative formulations for the obnoxious $p$-median problem"*, *Discrete Applied Mathematics*, 289, pp. 366–373.
* **T.L. Lei & R.L. Church (2015)**, *"On the unified dispersion problem: Efficient formulations and exact algorithms"*, *European Journal of Operational Research*, 241, pp. 622–630.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Mathematical Formulation

The unified model for dispersing facilities from clients is formulated as follows:

$$
\begin{align}
\max \quad & K \cdot t - \sum_{i \in I} u_i \\
\text{s.t.} \quad & t - u_i \leq q_i, && \forall i \in I \\
& q_i = L \cdot s_i - \sum_{j \in J} z(i,j), && \forall i \in I \\
& s_i - z(i,j) \leq d(i,j) + M(1 - y_j), && \forall i \in I, \; j \in J \\
& \sum_{j \in J} y_j = p, \\
& u_i \geq 0, \quad z(i,j) \geq 0, && \forall i \in I, \; j \in J \\
& y_j \in \{0, 1\}, && \forall j \in J \\
& t, s_i, q_i \in \mathbb{R}, && \forall i \in I
\end{align}
$$

#### Model Parameters & Variables

* **Sets**:
  * $I = \{1, \dots, n\}$: Set of client/demand locations.
  * $J = \{1, \dots, m\}$: Set of candidate facility sites.
* **Parameters**:
  * $d(i, j)$: Distance or undesirability measure between client $i$ and candidate facility $j$.
  * $p$: Exact number of facilities to open.
  * $K, L$: Dispersion tuning parameters. Setting $K = n$ and $L = 1$ aligns the unified model with the exact OpMP objective.
  * $M$: A sufficiently large positive scalar (Big-$M$) satisfying $M \geq \max_{i,j} d(i,j)$, ensuring logical activation when $y_j = 1$.
* **Decision Variables**:
  * $y_j \in \{0, 1\}$: Binary selection variable equal to $1$ if facility site $j$ is opened, $0$ otherwise.
  * $t \in \mathbb{R}$: Auxiliary variable representing the dispersion threshold for the $K$-th smallest distance.
  * $u_i \geq 0$: Auxiliary shortfall variable for each client $i$.
  * $q_i \in \mathbb{R}$: Connecting variable linking $t$, $u_i$, $s_i$, and $z(i,j)$.
  * $s_i \in \mathbb{R}$: Variable capturing the $L$-th smallest distance from client $i$ to any opened facility.
  * $z(i, j) \geq 0$: Slack variable active when facility $j$ is among the $L$ closest to client $i$.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Project Structure

```
exam_project_work/
├── IV_formulation.ipynb        # Complete Jupyter notebook with formulations, solver & tests
├── obnoxious-p-median-master/  # Benchmark dataset repository (Belotti et al., 2007)
│   ├── data/                   # Gzip-compressed Euclidean datasets (Hoefer, OR-Library)
│   ├── data-tr/                # Gzip-compressed distance matrix datasets (BildeKrarup, GalvaoRaggi, etc.)
│   ├── old_instances/          # Coordinate (.euc) and Table (.table) instance files
│   └── README.md               # Dataset format documentation
├── reference_paper/            # Academic background papers
│   ├── 1-s2.0-S0166218X2030490X-main.pdf               # Lin & Chiang (2021)
│   └── 1-s2.0-S0166218X2030490X-main (highlighted).pdf # Annotated paper copy
└── .gitignore                  # Local git ignore (excludes .idea, .venv, *.lp)
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

* [![Python][Python]][Python-url]
* [![Jupyter][Jupyter]][Jupyter-url]
* [![Gurobi][Gurobi]][Gurobi-url]
* [![NetworkX][NetworkX]][NetworkX-url]
* [![Matplotlib][Matplotlib]][Matplotlib-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

Follow these steps to set up and run this project folder.

### Prerequisites

* Python 3.10 or later
* Gurobi Optimizer with an active academic or commercial license (`gurobipy`)
* Jupyter Notebook or JupyterLab

### Installation

1. From the repository root, activate your virtual environment:
   ```sh
   # On Windows (PowerShell):
   ..\.venv\Scripts\Activate.ps1
   # On Linux / macOS:
   source ../.venv/bin/activate
   ```

2. Ensure all dependencies are installed:
   ```sh
   pip install notebook gurobipy networkx matplotlib
   ```

3. Confirm Gurobi license status:
   ```sh
   python -c "import gurobipy as gp; m = gp.Model(); print('Gurobi is ready!')"
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

Open the Jupyter notebook to run the interactive experiments:

```sh
jupyter notebook IV_formulation.ipynb
```

### 1. Toy Model & Graph Construction
The notebook begins with an intuitive toy instance:
* $3$ clients, $2$ candidate facility sites, $p = 1$.
* Generates 2D coordinates on a $100 \times 100$ plane.
* Constructs a bipartite graph $G = (V, E)$ connecting each client to candidate sites with Euclidean distances.

### 2. Gurobi Model Optimization
* Constructs the Gurobi optimization model using `gurobipy`.
* Adds variables ($y_j$, $t$, $u_i$, $q_i$, $s_i$, $z(i,j)$) and objective function $\max (K \cdot t - \sum u_i)$.
* Applies Big-M constraints dynamically.
* Solves to optimality using Gurobi's branch-and-cut engine.

### 3. Correctness Testing via Brute-Force
* Implements an exact brute-force algorithm using `itertools.combinations` to compute all $\binom{m}{p}$ possible facility subsets.
* Calculates the minimum distance to an opened facility for every client and sums them up.
* Automatically verifies that Gurobi's solution matches the exact combinatorial ground truth.

### 4. Benchmark Dataset Experiments
* Decompresses and parses `.gz` files from `obnoxious-p-median-master/`.
* Supports both Euclidean coordinate files and full distance table formats.
* Calculates adaptive Big-M bounds: $M = \max(d(i,j)) + 100$.
* Solves instances and plots optimal spatial configurations.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

This directory contains individual coursework developed for academic evaluation in the **Decision Optimization** course at the **University of L'Aquila (UnivAQ)**.

As this is a completed and archived university project, **contributions, pull requests, and feature requests are not accepted**. Consequently, no `CONTRIBUTING.md` file is maintained.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Contributors

<a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=CarlinoCalogero/univaq-decision-optimization" alt="contrib.rocks image" />
</a>

* **[Calogero Carlino](https://github.com/CarlinoCalogero)** &mdash; Master's Graduate in Computer Science (UnivAQ) & Game Programmer
  * GitHub: [@CarlinoCalogero](https://github.com/CarlinoCalogero)
  * LinkedIn: [Calogero Carlino](https://www.linkedin.com/in/calogero-carlino-2a94a33b5/)
  * Portfolio: [carlinocalogero.github.io](https://carlinocalogero.github.io)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

**Calogero Carlino**  
* Email: [carlinocalogero.dev@gmail.com](mailto:carlinocalogero.dev@gmail.com)
* LinkedIn: [linkedin.com/in/calogero-carlino-2a94a33b5/](https://www.linkedin.com/in/calogero-carlino-2a94a33b5/)
* Portfolio: [https://carlinocalogero.github.io](https://carlinocalogero.github.io)
* Repository: [https://github.com/CarlinoCalogero/univaq-decision-optimization](https://github.com/CarlinoCalogero/univaq-decision-optimization)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [University of L'Aquila (UnivAQ)](https://www.univaq.it/) &mdash; Department of Information Engineering, Computer Science and Mathematics (DISIM)
* Decision Optimization Course &mdash; Master's Degree in Computer Science
* [Chang-Chun Lin & Yen-I Chiang (2021)](https://doi.org/10.1016/j.dam.2020.11.002) &mdash; *Alternative formulations for the obnoxious $p$-median problem*, *Discrete Applied Mathematics*
* [Pietro Belotti, Martine Labbé, Francesco Maffioli, and Moustapha Ndiaye (2007)](https://doi.org/10.1007/s10288-006-0023-3) &mdash; Benchmark problem instances for the obnoxious $p$-median problem
* [Gurobi Optimization](https://www.gurobi.com/)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/CarlinoCalogero/univaq-decision-optimization.svg?style=for-the-badge
[contributors-url]: https://github.com/CarlinoCalogero/univaq-decision-optimization/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/CarlinoCalogero/univaq-decision-optimization.svg?style=for-the-badge
[forks-url]: https://github.com/CarlinoCalogero/univaq-decision-optimization/network/members
[stars-shield]: https://img.shields.io/github/stars/CarlinoCalogero/univaq-decision-optimization.svg?style=for-the-badge
[stars-url]: https://github.com/CarlinoCalogero/univaq-decision-optimization/stargazers
[issues-shield]: https://img.shields.io/github/issues/CarlinoCalogero/univaq-decision-optimization.svg?style=for-the-badge
[issues-url]: https://github.com/CarlinoCalogero/univaq-decision-optimization/issues
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/calogero-carlino-2a94a33b5/

<!-- Tech Badges -->
[Python]: https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white
[Python-url]: https://www.python.org/
[Jupyter]: https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white
[Jupyter-url]: https://jupyter.org/
[Gurobi]: https://img.shields.io/badge/Gurobi_Optimizer-EE3124?style=for-the-badge&logo=gurobi&logoColor=white
[Gurobi-url]: https://www.gurobi.com/
[NetworkX]: https://img.shields.io/badge/NetworkX-000000?style=for-the-badge&logo=networkx&logoColor=white
[NetworkX-url]: https://networkx.org/
[Matplotlib]: https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white
[Matplotlib-url]: https://matplotlib.org/
