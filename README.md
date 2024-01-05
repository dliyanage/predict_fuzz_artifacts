# Extrapolating Coverage Rate in Greybox Fuzzing (Artifacts)

For Artifact Evaluation at ICSE 2024.

<a href="https://github.com/dliyanage/predict_fuzz_artifacts/tree/main/paper/ICSE24_Extrapolation.pdf"><img src="https://github.com/dliyanage/predict_fuzz_artifacts/blob/main/ICSE24_Extrapolation.png" align="right" width="280"></a>

## Submission
* [Danushka Liyanage](https://dliyanage.github.io/) (Monash University, Australia) - Co-first author,
* [Seongmin Lee](https://nimgnoeseel.github.io/) (Max Planck Institute for Security and Privacy, Germany) - Co-first author,
* [Chakkrit Tantithamthavorn](https://chakkrit.com/) (Monash University, Australia),
* [Marcel Böhme](https://mboehme.github.io/) (Max Planck Institute for Security and Privacy)

```bibtex
@inproceedings{extrapolatecoverage,
 author = {Liyanage, Danushka and Lee, Seongmin and Tantithamthavorn, Chakkrit and B{\"o}hme, Marcel, 
 title = {Extrapolating Coverage Rate in Greybox Fuzzing},
 booktitle = {Proceedings of the 46th International Conference on Software Engineering (ICSE ’24), April 14–20, 2024, Lisbon, Portugal.},
 series = {ICSE},
 year = {2024},
 numpages = {13}
}
```

## Overview
A fuzzer can literally run forever. However, as more resources are spent, the coverage rate continuously drops and the utility the fuzzer declines. To tackle this coverage-resource tradeoff, we could introduce a policy to stop a campaign whenever the coverage rate drops below a certain threshold value, say 10 new branches covered per 15 minutes. During the campaign, can we predict the coverage rate at some point in the future? If so, how well can we predict the future coverage rate as the prediction horizon or the current campaign length increases? How can we tackle the statistical challenge of adaptive bias which is inherent in greybox fuzzing (i.e., samples are not independent and identically distributed)?

In this paper, we i) evaluate existing statistical techniques to predict the coverage rate $U(t_0+k)$ at any time $t_0$ in the campaign after a period of $k$ units of time in the future and ii) develop a new extrapolation methodology that tackles the {adaptive bias}.We propose to efficiently simulate a large number of blackbox campaigns from the collected coverage data, estimate the coverage rate for each of these blackbox campaigns and condunct a simple regression to extrapolate the coverage rate for the greybox campaign.

Our empirical evaluation using the Fuzztastic fuzzer benchmark demonstrates that, our extrapolation methodology exhibits at least one order of magnitude lower bias compared to the existing benchmark for $4$ out of $5$ experimental subjects we investigated Notably, compared to the existing extrapolation methodology, our extrapolator excels in making long-term predictions, such as those extending up to three times the length of the current campaign.


## Contents
We ensure the transparency and reproducibility of our paper's data, analysis, and results through this repository. Each directory contains the following content.

### **./scripts**

This is the directory where the data analysis Jupyter notebook resides. Analysis of data and preparation of all figures/tables of the paper happens within the R notebook namely `experimental.ipynb`. To run the notebook using raw data from experiments, you may set `REGENERATE_DATA = TRUE` which might take a while to execute. Or else, you can load the already gathered data from saved R workspaces by setting `REGENERATE_DATA = FALSE`. 

Please refer the *experimental.ipynb* notebook for more details. The compatible R environment details are provided in a later section.

**Running the Notebook -** We have provided instructions on setting up a compatible R environment, installing packages, and running the notebook in `./INSTALL.md`. 

### **./data**

This directory contains all the data used in the paper, including raw data from the conducted fuzzing campaigns.

***01. Raw Experimental Data -*** 

As explained in the `Experimental Setup` section of our paper, we executed the state-of-the-art fuzzer AFL++ on various candidate programs. The tool "*[Fuzztastic](https://ieeexplore.ieee.org/document/9793832)*" was used to conduct the experiments, producing coverage results in **.json** files at regular intervals. The interval size is customizable. Enclosed are the compressed (zip) files containing raw data:

* `fuzztastic_data_raw.zip` - Contains data from fuzzing campaigns on real-world programs (data volume ~ 31GB)

When the `REGENERATE_DATA` parameter is set to `TRUE`, the unzipping of these data files occurs automatically.

***02. R-data files -*** 

To reduce excessive run time in the Jupyter notebook (located under *./scripts*), we organize the necessary data into R data frames and save them as R workspaces. For example, the raw data from each greybox fuzzing trial is stored in the R data frame named `gb_data` and saved as `gb_data.Rdata`. We load them for data analysis and figure creation when the `REGENERATE_DATA` parameter is set to `FALSE`. Below is the list of all the `.Rdata` files resulting from our analysis:

* `gb_data.Rdata`       - Greybox fuzzing data
* `bb_data_*.Rdata`     - Split data file after performing the shuffling algorithm
* `extrapolated.Rdata`  - Extrapolation results for both our and Chao-Jost approaches
* `train_data_*.Rdata`  - Split data file for training datasets in performing extrapolation

Data files related to **RQ.2** are indicated as `*_sensitivity.Rdata`.

### **./figures**

Each output, including figures and tables, from `./scripts/experimental.ipynb` is saved in this directory.

### **./paper**

The preprint of the accepted paper can be found in this directory. The file is named `ICSE24_Extrapolation.pdf`.

## R-Environment and Package Details

We recommend using `R version 3.6.3` (release date: 2020-02-29) to run the Jupyter notebook `experimental.ipynb` located in the *./scripts* directory.

Below are the recommended R package versions:

```
* ggplot2       - 3.3.5
* tidyr         - 1.1.3
* tidyverse     - 1.3.1
* dplyr         - 1.0.7
* stringr       - 1.4.0
* scales        - 1.1.1
* RColorBrewer  - 1.1-2
* rjson         - 0.2.20
* jsonlite      - 1.7.2
* ggpubr        - 0.4.0
* cowplot       - 1.1.1
* gridExtra     - 2.3
* forcats       - 0.5.1
* gridExtra     - 2.3
* plotly        - 4.9.4.1
* foreach       - 1.4.8
* doParallel    - 1.0.16
* lsr           - 0.5.2
* rstatix       - 0.7.2
* colorspace    - 2.0-1
* viridis       - 0.6.1
* reshape2      - 1.4.3
```

Additionally, refer to `REQUIREMENTS.md` for more specific details on package requirements in the replication package.

## Running Fuzztastic

Fuzztastic is a fuzzer-agnostic coverage analyzer designed for uniform fuzzer evaluations. For specific details about Fuzztastic, refer to the following links:

* [Conference article](https://dl.acm.org/doi/10.1145/3510454.3516847)
* [Youtube video](https://www.youtube.com/watch?v=Lm-eBx0aePA&ab_channel=TUMSoftwareSystemsEngineering)

Here is an example of how to run Fuzztastic on a candidate program:

**Example:**
Assuming you are running `AFL++` on the target program `freetype2`, set the bash variables `subject="freetype2"`, `output_dir` (directory to save outputs), and `run` (trial number). Then,

1. Navigate to `./fuzztastic/fuzztastic-evaluations/docker/` directory.
2. Trigger the campaign using: `docker run --mount type=bind,source=${output_dir},target=/transfer fuzztastic/$subject/aflpp bash -c "/scripts/monitor_fuzzer.sh $subject aflpp c $Length /transfer $run ft-shm false false true"`


## Also Refer...

* [CONTACT.md](./CONTACT.md) - Contact details about the authors
* [INSTALL.md](./INSTALL.md) - Installation details to run R Jupyter notebook (An example)
* [STATUS.md](./STATUS.md)   - Message for the Artifact Evaluation Committee of ICSE'24
