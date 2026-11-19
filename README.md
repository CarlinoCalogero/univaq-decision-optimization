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
  <h3 align="center">UnivAQ &mdash; Decision Optimization</h3>

  <p align="center">
    <strong>Mathematical Formulation and Exact Solution of the Obnoxious <em>p</em>-Median Problem (OpMP)</strong>
    <br />
    <em>University of L'Aquila (UnivAQ) &bull; Master's Degree in Computer Science</em>
    <br />
    <br />
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/blob/main/exam_project_work/IV_formulation.ipynb"><strong>Explore the Notebook »</strong></a>
    <br />
    <br />
    <a href="https://github.com/CarlinoCalogero/univaq-decision-optimization/tree/main/exam_project_work">Project Work</a>
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
        <li><a href="#problem-overview">Problem Overview</a></li>
        <li><a href="#repository-structure">Repository Structure</a></li>
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
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

This repository contains the academic exam project work developed for the **Decision Optimization** course, part of the **Master's Degree in Computer Science (Advanced Software Engineering)** at the **University of L'Aquila (UnivAQ)**.

The focus of this project is the study, mathematical modeling, and computational resolution of the **Obnoxious $p$-Median Problem (OpMP)**, an essential combinatorial optimization challenge in operations research and discrete facility location.

### Problem Overview

Conventional facility location problems (such as the classical $p$-median or $p$-center problems) aim to minimize total travel distance or delivery costs between clients and desirable facilities (e.g., warehouses, service centers, hospitals). 

In contrast, **obnoxious facility location** addresses undesirable or hazardous facilities (such as chemical waste sites, garbage incinerators, nuclear facilities, or noisy airports) where proximity imposes negative environmental, safety, or health impacts on surrounding communities. The objective of the **Obnoxious $p$-Median Problem (OpMP)** is to select and open exactly $p$ facilities from a set of candidate locations such that their distance to clients is maximized (dispersing facilities away from client populations).

This project implements and tests the **Unified Model for dispersing facilities from clients** (analyzed as Formulation IV from the academic literature), based on the reference paper:
> **Chang-Chun Lin & Yen-I Chiang (2021)**, *"Alternative formulations for the obnoxious $p$-median problem"*, *Discrete Applied Mathematics*, Vol. 289, pp. 366–373.

Key highlights of this project:
* **Mathematical Modeling**: Formulated as a Mixed-Integer Linear Program (MILP) with continuous auxiliary variables for linearization and Big-M logical assignment constraints.
* **Toy Model & Graph Representation**: Interactive generation of client/facility 2D coordinates and bipartite network visualization using **NetworkX** and **Matplotlib**.
* **Brute-Force Exact Validation**: Implementation of an exhaustive combinatorial brute-force search algorithm to automatically verify the correctness of Gurobi's branch-and-cut optimal solution.
* **Benchmark Instance Parsing**: Automated extraction and parsing of `.gz` compressed datasets in Euclidean and Table distance-matrix formats from the benchmark archive of Belotti et al. (2007).
* **Modular Architecture**: Encapsulated dynamic model-building pipeline with adaptive Big-M calculation based on the instance's maximum distance metric.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Repository Structure

```
univaq-decision-optimization/
├── exam_project_work/                          # Main university exam project directory
│   ├── obnoxious-p-median-master/              # Benchmark dataset instances (Belotti et al., 2007)
│   │   ├── data/                               # Euclidean instances (Hoefer, OR-Library)
│   │   ├── data-tr/                            # Distance matrix instances (BildeKrarup, GalvaoRaggi, kmedian)
│   │   ├── old_instances/                      # Classic Euclidean (.euc) and Table (.table) instances
│   │   └── README.md                           # Dataset documentation
│   ├── reference_paper/                        # Theoretical background & literature
│   │   ├── 1-s2.0-S0166218X2030490X-main.pdf   # Lin & Chiang (2021) paper
│   │   └── 1-s2.0-S0166218X2030490X-main (highlighted).pdf
│   ├── .gitignore                              # Git ignore rules
│   ├── IV_formulation.ipynb                    # Jupyter Notebook containing complete model, solver & tests
│   └── README.md                               # Dedicated exam project README
├── BLANK_README.md                             # Template specification
├── site.json                                   # Author portfolio & profile metadata
└── README.md                                   # Root project documentation
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

Follow the steps below to set up the environment and run the project locally.

### Prerequisites

* **Python 3.10+**
* **Gurobi Optimizer & License**: A valid Gurobi license is required to run the solver. Free academic licenses are available for university students and researchers via the [Gurobi Academic Program](https://www.gurobi.com/academia/academic-program-and-licenses/).
* **Jupyter Notebook** or **JupyterLab**

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/CarlinoCalogero/univaq-decision-optimization.git
   cd univaq-decision-optimization
   ```

2. Create and activate a Python virtual environment:
   ```sh
   python -m venv .venv
   ```
   * On Windows (PowerShell):
     ```sh
     .venv\Scripts\Activate.ps1
     ```
   * On Linux / macOS:
     ```sh
     source .venv/bin/activate
     ```

3. Install required Python packages:
   ```sh
   pip install notebook gurobipy networkx matplotlib
   ```

4. Verify your Gurobi installation and academic license:
   ```sh
   grbgetkey <your-gurobi-license-token>
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

All models, solvers, and experimental validations are packaged inside the self-contained Jupyter Notebook located at `exam_project_work/IV_formulation.ipynb`.

1. Launch Jupyter Notebook from the project root:
   ```sh
   jupyter notebook exam_project_work/IV_formulation.ipynb
   ```

2. Follow the numbered sections in the notebook:
   * **Section 1: Toy Model**: Instantiates a 3-client, 2-facility problem ($p=1, K=n, L=1$) and plots the initial bipartite graph.
   * **Sections 2–6: Gurobi Model Creation**: Defines the MILP variables ($y_j, t, u_i, q_i, s_i, z_{ij}$), objective function, objective linkage constraints, $L$-th smallest distance definition, and Big-M assignment logic.
   * **Section 7: Solution Visualization**: Extracts the optimal facility locations and renders the bipartite network highlighting selected facilities.
   * **Section 8: Correctness Testing (Brute-Force)**: Runs an exact combinatorial search across all facility combinations to validate Gurobi's optimal objective and chosen sites.
   * **Section 9: Benchmark Dataset Testing**: Decompresses `.gz` instances from the Belotti et al. archive, parses coordinates and distance matrices, dynamically computes a valid Big-M bound, solves the problem, and displays the resulting layout.

_For further details on the specific mathematical formulation, see [`exam_project_work/README.md`](file:///c:/Users/calog/Documents/univaq-decision-optimization/exam_project_work/README.md)._

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

This repository contains individual coursework submitted for academic evaluation in the **Decision Optimization** course at the **University of L'Aquila (UnivAQ)**. 

Because this is a completed and archived university project, **external contributions, pull requests, and feature requests are not accepted**. Consequently, no `CONTRIBUTING.md` guide is maintained.

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
* Project Link: [https://github.com/CarlinoCalogero/univaq-decision-optimization](https://github.com/CarlinoCalogero/univaq-decision-optimization)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [University of L'Aquila (UnivAQ)](https://www.univaq.it/) &mdash; Department of Information Engineering, Computer Science and Mathematics (DISIM)
* Decision Optimization Course &mdash; Master's Degree in Computer Science (Advanced Software Engineering)
* [Chang-Chun Lin & Yen-I Chiang (2021)](https://doi.org/10.1016/j.dam.2020.11.002) &mdash; *Alternative formulations for the obnoxious $p$-median problem*, *Discrete Applied Mathematics*
* [Pietro Belotti, Martine Labbé, Francesco Maffioli, and Moustapha Ndiaye (2007)](https://doi.org/10.1007/s10288-006-0023-3) &mdash; Benchmark problem instances for the obnoxious $p$-median problem
* [Gurobi Optimization](https://www.gurobi.com/) &mdash; Mixed Integer Linear Programming Solver
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template) &mdash; README documentation template

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
