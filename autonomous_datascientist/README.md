# Autonomous Data Scientist (ADaS): A Practical Example of the Future Role of AI in Cybersecurity
## Abstract

The volume of network traffic and elusiveness of modern cyber threats challenge the ability of cyber analysts to identify anomalous behavior in networks. In this work, we propose an autonomous agent to enhance analysts’ detection capabilities. To this end, we designed an agent to analyze feature-rich network traffic by autonomously executing the Data Analytic Development Process: (1) data engineering–prepares datasets, (2) feature engineering–selects the most representative features, (3) model engineering–chooses the best algorithm-feature combination, and (4) analytic deployment–applies the optimized analytic to the dataset and identifies anomalous behavior. We refer to this agent as an Autonomous Data Scientist (ADaS), utilizing reinforcement learning as an orchestrator to determine the optimal combination of features and unsupervised clustering algorithm. ADaS operates without labeled training data, and results on popular public datasets (NB15, IoT-23, and KDD’99) demonstrate high detection and low false alarm rates, making it reliable and novel for anomaly detection.

This work is published as part of the The 50th IEEE Conference on Local Computer Networks (LCN). Please see [here](https://ieeexplore.ieee.org/document/11146345) for the official publication: [https://ieeexplore.ieee.org/document/11146345](https://ieeexplore.ieee.org/document/11146345)


## Repository Guide
### ADaS Codebase
ADaS autonomously executes the Data Analytic Development Process, with code organized as follows: 
1. Data Engineering – prepares datasets.
2. Feature Engineering – selects the most representative features.
- Implemented in [notebooks/Preprocessing and Feature Selection.ipynb](https://github.com/simonegreen/autonomous_datascientist/blob/main/notebooks/Preprocessing%20and%20Feature%20Selection.ipynb).
3. Model Engineering – chooses the best algorithm-feature combination.
4. Analytic Deployment – applies the optimized analytic to network traffic data to identify anomalous behavior.
- Implemented in [notebooks/Reinforcement Learning.ipynb](https://github.com/simonegreen/autonomous_datascientist/blob/main/notebooks/Reinforcement%20Learning.ipynb).

### Deterministic Variant
- The setup for our controlled comparison (Experiment 2: ADaS vs. Deterministic Variant) is available in [notebooks/Deterministic Notebook.ipynb](https://github.com/simonegreen/autonomous_datascientist/blob/main/notebooks/Deterministic%20Notebook.ipynb).

### Datasets
- Sampled subsets of NB15, IoT-23, and KDD’99 datasets (used for experiments to balance data retention and computational efficiency) are available in the [data](data) folder.

### Past Trials
- Previous versions of datasets and notebooks are archived in [past-trials](past-trials).
