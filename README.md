## Dynamic Network Cascade and COVID-19 Simulation
---
Oanh Tran
William Trinh

## Project Description

This project implements two network-based diffusion models using Python, NetworkX, and Matplotlib:

1. **Cascade Simulation** – A Linear Threshold Model that demonstrates how influence spreads through a network.
2. **COVID-19 Simulation** – A modified SIRS epidemiological model with vaccination, shelter-in-place behavior, immunity waning, and mortality.

The program reads a graph in GML format and simulates how activation or disease propagates across the network. 

---

## Features


### COVID-19/Disease-Spread Model
- Modified SIRS model
- Vaccination with reduced infection probability
- Recovery or death after 10 days of infection
- Immunity that wanes over time
- Shelter-in-place behavior
- Final network visualization
- Optional bar chart of infections per day

<img width="1602" height="1358" alt="image" src="https://github.com/user-attachments/assets/b81ae824-e2f9-4301-a587-c35a7a73f89e" />
<img width="1366" height="754" alt="image" src="https://github.com/user-attachments/assets/2c42b5de-b7f9-4d5f-bf66-aba594978a5e" />
<img width="936" height="700" alt="image" src="https://github.com/user-attachments/assets/06a82ec0-b386-4b0f-a858-870f14b48baf" />


---
## Project Assumptions, Modifications, and SIRS Explanation

---

### What Can Be Modified

Several parameters can be modified from the command line to test different scenarios.

The `--probability_of_infection` parameter controls how easily the disease spreads from infected nodes to susceptible neighbors. Increasing this value causes faster and wider spread.

The `--probability_of_death` parameter controls the chance that an infected node dies after being infected for 10 days. A higher value results in more deaths and fewer recoveries.

The `--vaccination` parameter controls the proportion of non-initiator nodes that begin vaccinated. Increasing vaccination generally reduces infections because vaccinated nodes have a lower infection probability.

The `--shelter` parameter controls both the proportion of initially sheltered nodes and the likelihood that susceptible nodes enter shelter when many of their neighbors are infected. Increasing this value can slow disease spread.

The `--lifespan` parameter controls how many days the COVID simulation runs. A longer lifespan allows more time for infection, recovery, death, and immunity waning.

The `--initiator` parameter controls which nodes begin infected in the COVID model or active in the cascade model. Changing the initiators can significantly change the outcome because spread depends on the network structure.

The graph file itself can also be modified. Adding or removing nodes and edges changes how connected the network is, which affects both cascade activation and disease transmission.

---


### How the SIRS Model Works

The COVID simulation is based on the SIRS model, which stands for:

- Susceptible
- Infected
- Recovered
- Susceptible

A susceptible node is healthy but can become infected. An infected node can spread the disease to its neighbors. A recovered node is temporarily immune. After the immunity period ends, the recovered node becomes susceptible again, meaning it can be infected in the future.

In this project, the SIRS model is extended with additional states:

| State | Meaning |
|------|---------|
| `S` | Susceptible |
| `I` | Infected |
| `R` | Recovered |
| `D` | Dead |
| `SH` | Sheltered |

Each day, the simulation takes a snapshot of the current network state. All changes for that day are calculated from this snapshot, then applied at the end of the day. This is called a synchronous update. It prevents unrealistic same-day chain infections where a newly infected node immediately infects another node on the same day.

The infection process begins with one or more initiator nodes. These nodes start in the infected state. Each infected node checks its neighbors. If a neighbor is susceptible, it may become infected based on the infection probability. If the neighbor is vaccinated, it may still become infected, but with a reduced probability.

An infected node remains infected for 10 days. After 10 days, the node either dies or recovers. The chance of death is controlled by `--probability_of_death`. If the node does not die, it becomes recovered.

Recovered nodes have temporary immunity. In this project, immunity lasts:
- 90 days for recovered unvaccinated nodes
- 180 days for vaccinated nodes that were never infected
- 365 days for vaccinated nodes that recovered after infection

After immunity expires, the node returns to the susceptible state. This is what makes the model SIRS instead of SIR. In an SIR model, recovered nodes stay recovered forever. In an SIRS model, recovered nodes can eventually become susceptible again.

Shelter-in-place adds another layer to the model. If a susceptible node has more than 50% infected neighbors, it may enter the sheltered state. Sheltered nodes remain sheltered for 14 days and then return to susceptible.

Overall, the model simulates a simplified example of how disease can spread, slow down, recover, and potentially return over time in a connected network.

### Project Assumptions

This project makes several assumptions to simplify the simulation while still modeling realistic network-based spread.

For the COVID model, the simulation assumes that disease spreads only through connected neighbors in the graph. A node can only infect another node if there is an edge between them. The model also assumes that all initiator nodes begin infected, while vaccinated and sheltered nodes are selected randomly from the remaining non-initiator nodes.

The COVID model assumes that vaccinated nodes are not infected at the start, but they can still experience breakthrough infections. Vaccination lowers the infection probability rather than making a node completely immune. Recovered nodes are temporarily immune and cannot be reinfected until their immunity expires. Dead nodes remain dead for the rest of the simulation and do not infect or recover.

Sheltered nodes are assumed to avoid infection while sheltered. Shelter lasts for 14 days, after which the node returns to the susceptible state. Susceptible nodes may enter shelter if more than half of their neighbors are infected.

## Requirements

Install the required packages:

```bash
pip install networkx matplotlib
