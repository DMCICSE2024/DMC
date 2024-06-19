README

# EMC

A Robust Windows Malware Classification Refinement for Concept Drift.


## Update after Rebuttal
FPR for all MalwareBazaar experiment (TABLE III)
|    Model     |    FPR    |
| :----------: | :-------: | 
|  ResNet-50   |    3.31   |
|    VGG-16    |    3.22   |
| Inception-V3 |    4.98   |
|    IMCFN     |    2.88   |
|   CBOW+MLP   |    1.73   |
|   MalConv    |    3.99   |
|    MAGIC     |   15.34   |
| Word2Vec+KNN |    7.08   |
|     MCSC     |    5.11   |
|     EMC      |    0.82   |

FPR for all MalwareDrift experiment (pre-drift in TABLE IV)
|    Model     |    FPR    |
| :----------: | :-------: | 
|    IMCFN     |   15.89   |
|   MalConv    |   29.33   |
|   CBOW+MLP   |   17.71   |
|    MAGIC     |   15.50   |
| Word2Vec+KNN |   22.31   |
|     MCSC     |   28.44   |
|     EMC      |   18.68   |

FPR for all MalwareDrift experiment (post-drift in TABLE IV)
|    Model     |    FPR    |
| :----------: | :-------: | 
|    IMCFN     |   60.44   |
|   MalConv    |   79.58   |
|   CBOW+MLP   |   92.31   |
|    MAGIC     |   74.14   |
| Word2Vec+KNN |   59.97   |
|     MCSC     |   61.08   |
|     EMC      |   37.77   |



## Setup

* `IDA Pro`: >= 7.5
* `pytorch`==1.7.0 
* `cudatoolkit`=11.1
* `datasets`==1.18.3
* `transformers`==4.16.2
* `tensorboard`==2.8.0

## Dataset

The dataset `MalwareBazaar` `MalwareDrift` and label files `MalwareBazaar_Labels.csv` `MalwareBazaar_Labels.csv` used in this paper and code come from the following paper.

\[FSE2021\] [A Comprehensive Study on Learning-Based PE Malware Family Classification Methods.](https://dl.acm.org/doi/abs/10.1145/3468264.3473925)

Dataset:<https://github.com/MHunt-er/Benchmarking-Malware-Family-Classification>


## Experimental Settings

|    Model     | Optimizer | Learning Rate | Batch Size |                     Input Format                      |
| :----------: | :-------: | :-----------: | :--------: | :---------------------------------------------------: |
|  ResNet-50   |   Adam    |     1e-3      |     64     |                  224*224 color image                  |
|    VGG-16    |    SGD    |    5e-6**     |     64     |                  224*224 color image                  |
| Inception-V3 |   Adam    |     1e-3      |     64     |                  224*224 color image                  |
|    IMCFN     |    SGD    |    5e-6***    |     32     |                  224*224 color image                  |
|   CBOW+MLP   |    SGD    |     1e-3      |    128     |       CBOW: byte sequences; MLP: 256*256 matrix       |
|   MalConv    |    SGD    |     1e-3      |     32     |                  2MB raw byte values                  |
|    MAGIC     |   Adam    |     1e-4      |     10     |                         ACFG                          |
| Word2Vec+KNN |     -     |       -       |     -      | Word2Vec: Opcode sequences; KNN distance measure: WMD |
|     MCSC     |    SGD    |     5e-3      |     64     |                   Opcode sequences                    |
|     EMC      |   Adam    |     5e-5      |     12     |                Orient-opcode sequences                |

Early Stopping Patience: 10


## Useage (Update after Rebuttal)

Disassembly


Put the `auto_opcode.py`  `opcode_extraction.py` in the IDA Pro working directory. 

Put the raw malware binary files in the `pefile_dir` and run
```
python auto_opcode.py
```

Feature Process

Put the output of `auto_opcode.py` in the `folder_path` and run
```
python opc_trans_sen.py
```

Model Training and Validation

Put the training and verification feature vectors processed by `opc_trans_sen.py` under path `traindir` `valdir`, and run
```
python main_FeatDe.py
```

