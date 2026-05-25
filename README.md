
# FedAnil+++: A Robust and Privacy-Preserving Personalized Federated Learning Framework for Intelligent Enterprises

<p align="justify">
FedAnil+++ is a robust, privacy-preserving, and communication-efficient personalized federated learning framework designed for intelligent enterprise environments with heterogeneous non-IID data. The framework integrates adaptive gradient compression, CKKS-based homomorphic encryption, blockchain-assisted secure aggregation, similarity-aware validation, and adversarial personalization to improve robustness against poisoning and inference attacks while reducing communication and computational overhead. This repository provides the official Python-based simulation and implementation of FedAnil+++.
</p>

## Introduction
<p align="justify">
Federated learning enables collaborative model training across enterprises without sharing raw private data. However, existing federated learning frameworks still suffer from privacy leakage, communication overhead, vulnerability to poisoning and inference attacks, and degraded performance under heterogeneous non-IID data distributions. To address these challenges, we propose FedAnil+++, a robust and privacy-preserving personalized federated learning framework for intelligent enterprises. The proposed framework consists of three main phases: (i) communication-efficient gradient compression, (ii) privacy-preserving and secure aggregation using CKKS-based homomorphic encryption, blockchain, and similarity-aware validation, and (iii) personalized adaptation for heterogeneous non-IID environments through clustering and adversarial learning.
</p>



## FedAnil++ Installation
## Requirements

### OS

| Windows | Linux | MacOS |
|:--:|:--:|:--:|
| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

### Python

| `3.9` | `3.10` | `3.11` | `3.12` |
| :------:|:-------:|:-------:|:-------:|
| :x: | :heavy_check_mark: | :x: | :x: |

### PyTorch

| `2.1.1` | `2.1.2` | `2.2.0` | `2.2.1` |
| :------:|:-------:|:-------:|:-------:|
| :x: | :x: | :x: | :heavy_check_mark: |

#### Step 1: Download the repo
```
git clone https://github.com/rezafotohi/FedAnilPlusPlusPlus.git
cd FedAnilPlusPlusPlus
```

#### Step 2: Create a new conda environment with Python 3.10
```
conda create -n FedAnil++ python=3.10
conda activate FedAnil++
```

#### Step 3: Install PyTorch and Jupyter
```
conda install pytorch torchvision torchaudio -c pytorch
conda install -c conda-forge jupyter jupyterlab
```

#### Step 4: Is the torch installed successfully? Enter the following commands in the terminal:
```
python3
import torch
```

#### Step 5: Install Pycryptodome and Matplotlib
```
conda install pycryptodome
conda install matplotlib
```

#### Step 6: Install Scikit-Learn
```
pip3 install scikit-learn-extra
```

#### Step 7: Install Bitarray
```
pip3 install bitarray
```

#### Step 8: Install TenSEAL
```
pip3 install git+https://github.com/OpenMined/TenSEAL.git#egg=tenseal
```

#### Step 9: Install CMake
On Windows and Linux:
```
Download the latest CMake Mac binary distribution here: https://cmake.org/download/
```
On MacBooks with M1 processor:
```
arch -arm64 brew install cmake
```

#### Step 10: Run FedAnil+++ Simulation
```
python3 main.py -nd 100 -max_ncomm 50 -ha 80,10,10 -aio 1 -pow 0 -ko 5 -nm 3 -vh 0.08 -cs 0 -B 64 -mn OARF -iid 0 -lr 0.01 -dtx 1 -le 20
```

## Arguments:

<b>-nd 100</b>: 100 Enterprises.

<b>-max_ncomm 50</b>: Maximum 50 communication rounds.

<p align="justify"> <b>-ha 80,10,10</b>: Role assignment hard-assigned to 80 workers, 10 validators, and 10 miners for each communication round. A <b>*</b> in <b>-ha</b> means the corresponding number of roles is not limited. e.g., <b>-ha *,10,*</b> means at least 5 validators are assigned in each communication round, and the remaining enterprises are dynamically and randomly assigned to any role. <b>-ha *,*,*</b> means the role-assigning in each communication round is completely dynamic and random. </p>

<p align="justify"> <b>-aio 1</b>: <i>aio</i> means "all in one network", namely, every enterprise in the simulation has every other enterprise in its peer list. This simulates FedAnil++ running on a Permissioned blockchain (consortium blockchain). If using <b>-aio 0</b>, the simulation will let an enterprise (registrant) randomly register with another enterprise (register) and copy the register's peer list. </p>

<p align="justify"> <b>-pow 0</b>: The argument of <b>-pow</b> specifies the proof-of-work difficulty. When using 0, FedAnil++ runs with FedAnil++-PoS consensus to select the winning miner. </p>

<b>-ko 5</b>: This argument means an enterprise is blacklisted after it is identified as malicious after six consecutive rounds as a worker.

<b>-nm 3</b>: Exactly 3 enterprises will be malicious nodes.

<p align="justify"> <b>-vh 0.08</b>: Validator-threshold is set to 0.08 for all communication rounds. Validators may adaptively learn this value in a future version. </p>

<p align="justify"> <b>-cs 0</b>: As the simulation does not include mechanisms to disturb the digital signature of the transactions, this argument turns off signature checking to speed up the execution. </p>

Federated Learning arguments (inherited from https://github.com/WHDY/FedAvg)

<b>-B 64</b>: Batch size set to 64.

<b>-mn OARF</b>: Use OARF Dataset.

<b>-iid 0</b>: Shard the training data set in Non-IID way.

<b>-lr 0.01</b>: Learning rate set to 0.01.

Other arguments

<b>-dtx 1</b>: See <b>Issues</b>.

Please see <i>main.py</i> for other argument options.

## Simulation Logs
#### Examining the Logs
<p align="justify"> While running, the program saves the simulation logs inside the <i>log/\<execution_time\></i> folder. The logs are saved per communication round. In the corresponding round folder, you may find the model accuracy evaluated by each enterprise using the global model at the end of each communication round. You may also find each worker's local training accuracy, the validation-accuracy difference for each validator, and the final stake awarded to each enterprise in this communication round. You may also find the malicious enterprise's identification log outside the round folders. </p>

## Issues
<p align="justify"> If you use a GPU with less than 16 GB of RAM, you may encounter the issue of <b>CUDA out of memory</b>. This issue may be due to local model updates (i.e., neural network models) stored in blocks that occupy CUDA memory and are not automatically released, as CUDA memory usage increases with each communication round. A few solutions have been tried but have not been successful. </p>

<p align="justify"> A temporary solution is to specify <b>-dtx 1</b>. This argument allows the program to delete the transactions stored in the last block to free as much CUDA memory as possible. However, specifying <b>-dtx 1</b> will also disable chain-resyncing functionality, as the resyncing process requires enterprises to reapply global model updates based on transactions stored in the resynced chain, which contains empty blocks. As a result, using a GPU should only emulate the situation in which FedAnil++ runs under its ideal conditions; that is, every available transaction would be recorded in the block of each round, as specified by the default arguments. </p>

Use [GitHub issues](https://github.com/tensorflow/federated/issues) for tracking
requests and bugs.

## Citation

If you publish work that uses FedAnil+++, please cite FedAnil+++ as follows:

```bibtex
@article{2026FedAnil+++,
  title = {Privacy-Preserving Personalized Federated Learning Against Poisoning and Inference Attacks in Intelligent Enterprises},
  author = {Reza Fotohi and Fereidoon Shams Aliee and Bahar Farahani},
  journal= {Under Review!},
  volume = {},
  pages = {},
  year = {2026},
  issn = {},
  doi = {},
  url = {},
}
```

## Disclaimer

This model is a research work and is provided as it is. We are not responsible for any user action or omission.

## Contact
Please don't hesitate to raise any other issues and concerns you may have. Thank you!

Email: Fotohi.reza@gmail.com

LinkedIn: https://www.linkedin.com/in/reza-fotohi-b433a169/

## Acknowledgments
(1) The code of the Blockchain Architecture used in FedAnil++ is inspired  [*Fully functional blockchain application implemented in Python from scratch*](https://github.com/satwikkansal/python_blockchain_app) by Satwik Kansal.

(2) The code of the Validation and Consensus scheme used in FedAnil++ is inspired [*VBFL*](https://github.com/hanglearning/VBFL) by Hang Chen.

(3) The code of the FedAvg used in FedAnil is inspired [*WHDY's FedAvg implementation*](https://github.com/WHDY/FedAvg) by WHDY.
