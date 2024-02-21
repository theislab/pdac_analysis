# Scripts and results for the paper "Inherent inter- and intratumoral heterogeneity drives phenotypic plasticity and treatment response of pancreatic cancer organoids"

The repository contains scripts to generate the paper figures. The analysis and repository is based on [Luecken, Malte D., and Fabian J. Theis. "Current best practices in single‐cell RNA‐seq analysis: a tutorial"](https://github.com/theislab/single-cell-tutorial).

## Data

The two adata objects used for the final analysis in the paper are available at [Zenodo](https://zenodo.org/records/10666382).

## Structure of the repository

In general, 4 tasks and preprocessing are considered:

- `Task0`: Preprocessing
- `Task1`: Differentially expressed genes across clusters
- `Task2`: GSEA Hallmark analysis for each cluster
- `Task3`: Epithelial to Mesenchymal (EMT) score
- `Task4`: Correlation of the clusters with the bulk RNA seq analysis

In the `data/` directory the raw as well as processed data, and the gene sets are stored.
The results, i.e. the paper figures, for the respective tasks are stored in the `results/` directory.
The notebooks are in the `notebooks/` directory.

In case of questions or issues, please get in touch by posting an issue in this repository.

## Environment set up

To set up a conda environment, the following instructions must be followed.

1. Set up the conda environment from the `environment.yml` file.

    ```
    conda env create -f environment.yml
    ```

2. Ensure that the environment can find the `gsl` libraries from R. This is done by setting the `CFLAGS` and `LDFLAGS` environment variables (see https://bit.ly/2CjJsgn). Here we set them so that they are correctly set every time the environment is activated.

    ```
    cd YOUR_CONDA_ENV_DIRECTORY
    mkdir -p ./etc/conda/activate.d
    mkdir -p ./etc/conda/deactivate.d
    touch ./etc/conda/activate.d/env_vars.sh
    touch ./etc/conda/deactivate.d/env_vars.sh
    ```

    Where YOUR_CONDA_ENV_DIRECTORY can be found by running `conda info --envs` and using the directory that corresponds to your conda environment name (default: da_env).

    WHILE NOT IN THE ENVIRONMENT open the `env_vars.sh` file at `./etc/conda/activate.d/env_vars.sh` and enter the following into the file:

    ```
    #!/bin/sh
    
    CFLAGS_OLD=$CFLAGS
    export CFLAGS_OLD
    export CFLAGS="`gsl-config --cflags` ${CFLAGS_OLD}"
     
    LDFLAGS_OLD=$LDFLAGS
    export LDFLAGS_OLD
    export LDFLAGS="`gsl-config --libs` ${LDFLAGS_OLD}"
    ```
    
    Also change the `./etc/conda/deactivate.d/env_vars.sh` file to:

    ```
    #!/bin/sh
     
    CFLAGS=$CFLAGS_OLD
    export CFLAGS
    unset CFLAGS_OLD
     
    LDFLAGS=$LDFLAGS_OLD
    export LDFLAGS
    unset LDFLAGS_OLD
    ```
    
    Note again that these files should be written WHILE NOT IN THE ENVIRONMENT. Otherwise you may overwrite the CFLAGS and LDFLAGS environment variables in the base environment!

3. Enter the environment by `conda activate da_env` or `conda activate ENV_NAME` if you changed the environment name in the `environment.yml` file.

4. Open R and install the dependencies via the commands:

    ```
    install.packages(c('devtools', 'gam', 'RColorBrewer', 'BiocManager'))
    update.packages(ask=F)
    BiocManager::install(c("scran","MAST","monocle","ComplexHeatmap","slingshot"), version = "3.8")
    ```
   
This environment is based on the setup of [SCIB](https://github.com/theislab/scib-pipeline), their installation guide can also be followed with the additions:
- pip install XlsxWriter
- pip install git+https://github.com/vitkl/orthologsBioMART.git
- pip install openpyxl
- in R: BiocManager::install("fgsea")
- in R: BiocManager::install("AUCell")


