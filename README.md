# XTPrecip

**A multisource spatiotemporal deep-learning framework for high-resolution precipitation estimation and cross-region transfer learning**

**Research materials and large-file archive:** [XTPrecip on Open Science Framework (OSF)](https://osf.io/vbr7h/)

XTPrecip is a deep-learning framework developed to estimate high-resolution hourly precipitation by integrating complementary gridded data sources and rain-gauge observations. It supports spatiotemporal model training, evaluation of alternative predictor configurations, and transfer learning between regions with different hydroclimatic characteristics.

This GitHub repository is intended to organize the research code and supporting materials for the **XTPrecip study**, including scripts for constructing station-centered spatiotemporal inputs, experiments conducted in the Appalachian study area, and model fine-tuning experiments at three transfer-learning locations. The associated research materials—including station data, selected trained models, Jupyter notebooks used to create paper figures, and large files—are available in the **[XTPrecip OSF project](https://osf.io/vbr7h/)**. Use the OSF record as the primary location for the complete deposited research files and this GitHub repository for code browsing, documentation, and future code updates.

> **Scope:** XTPrecip is the modeling framework described in the associated research paper. **ML4HP-US** is a separate long-term, CONUS precipitation dataset developed using this framework; its dataset documentation and access information should be maintained separately.

## Repository structure

```text
XTPrecip/
├── Appalachian/            # Appalachian model-development experiments (XTP1–XTP11)
│   └── ...                 # Scenario-specific scripts, selected models, and notebooks
├── transfer_learning/      # Model adaptation in three transfer-learning regions
│   ├── Brazil/             # Includes Brazil-specific window-assembly code
│   └── ...                 # Other regional experiment subfolders
├── window_assemble/        # Station-centered spatiotemporal data preparation
│   └── ...                 # Python scripts and accompanying station data
├── README.md               # Repository overview and reproducibility notes
└── LICENSE                 # Repository software license
```

*The tree summarizes the main directory roles in the local/GitHub code organization. For the deposited research files and any larger associated datasets or model files, visit the [OSF project](https://osf.io/vbr7h/). Individual filenames and OSF folder layout may differ from the outline above.*

## 1. Data preparation: `window_assemble/`

This folder contains Python scripts used to construct **station-centered spatiotemporal input cubes** from the gridded datasets included in the paper. These cubes provide the spatial and temporal context needed to train and evaluate XTPrecip at individual rain-gauge stations.

The folder also includes station observation data used in the experiments. The preprocessing workflow is designed to:

1. Read and align the required gridded predictor datasets.
2. Locate the target stations on the analysis grid.
3. Extract spatial neighborhoods across the required temporal window around each station.
4. Assemble multivariable spatiotemporal samples and align them with gauge-based precipitation targets.
5. Prepare inputs for the model-development and transfer-learning experiments.

**Reproducibility note:** The exact predictor combinations, temporal windows, input paths, and output formats depend on the scripts and configuration of each experiment. Input gridded datasets may need to be obtained separately from their original providers; including station observations in this repository does not imply that every large gridded input is redistributed here.

## 2. Model development in the Appalachian region: `Appalachian/`

The Appalachian study area is used for developing and evaluating XTPrecip. The paper examines **11 experimental scenarios, XTP1–XTP11**, which investigate alternative model/input configurations.

### Experiments and model stochasticity

- **11 scenarios:** XTP1 through XTP11, as defined in the paper.
- **Five independent runs per scenario:** The research experiments were repeated with random seeds **41, 42, 43, 44, and 45** to account for stochasticity in model training.
- **Reproducible example:** The deposited experiment materials provide model-training code configured with **seed 41** for each scenario. To reproduce the other runs, change the random-seed setting in the relevant script from `41` to `42`, `43`, `44`, or `45` and rerun that scenario.
- **Selected models:** The best model selected for each scenario is included in the research materials deposited on [OSF](https://osf.io/vbr7h/).

> The seed-41 scripts are examples of the run configuration. They do **not** mean that all five trained runs or all five checkpoints are included in the released materials. The selection criterion for the provided best model should be checked against the methods section of the paper.

### Training and validation data

Training inputs for **XTP1–XTP10** are large and are not included in the ordinary GitHub code repository. The **XTP11** scenario has a comparatively smaller training and validation dataset, but it still exceeds **35 GB**. The XTP11 training and validation data are available through the **[XTPrecip OSF archive](https://osf.io/vbr7h/)** as part of the deposited research materials.

**XTP11 data access:** Visit [https://osf.io/vbr7h/](https://osf.io/vbr7h/) and browse the project files for the XTP11 data. The exact OSF file paths and any download instructions should be checked on the project page.

For **XTP1–XTP10**, use the supplied `window_assemble/` and scenario scripts to regenerate training inputs when full preprocessed data are not included among the deposited files.

### Figure generation

Jupyter notebooks are included in the deposited research materials to reproduce several figures in the associated paper. Review each notebook's data and model file paths before execution; local file paths and software environments may need to be adapted.

## 3. Cross-region transfer learning: `transfer_learning/`

This folder contains experiments for **three transfer-learning locations** discussed in the paper. Regional subfolders hold the scripts and supporting materials for fine-tuning XTPrecip models in the target regions and evaluating regional adaptation.

- **Fine-tuning examples:** A seed-**41** fine-tuning run is provided for each transfer-learning experiment. To examine variability, the seed may be changed to **42–45**, where the corresponding workflow supports repeated runs.
- **Brazil preprocessing:** The `Brazil/` subfolder contains additional **window-assembly scripts** specific to the Brazil study region.
- **Paper figures:** Jupyter notebooks are supplied for generating figures associated with the transfer-learning experiments.

Refer to each regional subfolder and the paper for the applicable source model, station partition, fine-tuning settings, input datasets, and evaluation procedure. Regional experiments may not use identical data availability or preprocessing conventions.

## 4. Accessing the archived research files

The accompanying code, notebooks, station observations, selected trained models, and large supporting files are deposited on **Open Science Framework (OSF)**:

**[XTPrecip research archive — https://osf.io/vbr7h/](https://osf.io/vbr7h/)**

For a complete reproduction workflow, obtain the relevant files from OSF, then use the scripts and notebooks described in the sections above. The GitHub repository provides a browsable source-code entry point, while OSF provides access to the archived research materials, including files too large for ordinary GitHub commits. Individual file names, archive organization, and the version of each file should be checked on the OSF record.

## 5. Suggested reproduction workflow

1. **Identify the experiment.** Choose an Appalachian scenario (`XTP1`–`XTP11`) or a region under `transfer_learning/`.
2. **Review its inputs.** Check the relevant script/notebook and the paper for the predictor configuration, station lists, and required gridded datasets.
3. **Prepare station-centered samples.** Use the appropriate scripts in `window_assemble/` (or the Brazil-specific scripts for that regional study), unless preprocessed training data are provided.
4. **Set the random seed.** The supplied example scripts use `41`. For stochastic repeat runs, use `42`, `43`, `44`, and `45` as appropriate.
5. **Train or fine-tune.** Run the experiment-specific script using the software environment and computational resources required by that experiment.
6. **Evaluate and plot.** Use the included evaluation/figure notebooks and the selected trained model for comparison with the paper's results.

**Before running:** Check absolute data paths, station-data availability, software versions, preprocessing decisions, checkpoint paths, and CPU/GPU resources. This top-level README does not prescribe a single installation or execution command because the exact script names and dependencies have not yet been verified from the uploaded source folders.

## 6. Model and data availability

| Resource | Availability / location |
| --- | --- |
| Research-materials archive | **[OSF: XTPrecip](https://osf.io/vbr7h/)** |
| Spatiotemporal window assembly scripts | `window_assemble/`; deposited materials on OSF |
| Station observations used in window assembly | Provided with the window-assembly research materials on OSF (observe original data-use terms) |
| Appalachian scenario scripts (XTP1–XTP11) | `Appalachian/`; deposited materials on OSF |
| Example training run for each Appalachian scenario | Seed `41`; code in deposited experiment materials |
| Additional stochastic runs | Reproduce by changing the seed to `42–45` |
| Selected best-performing model for each Appalachian scenario | Deposited with experiment materials on OSF |
| XTP1–XTP10 full training datasets | Not included in the ordinary GitHub repository; regenerate using supplied preprocessing scripts where needed |
| XTP11 training and validation data | Over 35 GB; **available in the [OSF archive](https://osf.io/vbr7h/)** |
| Transfer-learning scripts | `transfer_learning/`; deposited materials on OSF |
| Transfer-learning fine-tuning examples | Seed `41`; code in deposited experiment materials |
| Brazil-specific window assembly | `transfer_learning/Brazil/`; deposited materials on OSF |
| Figure reproduction notebooks | Within the associated experiment materials on OSF |

**Large files:** The [OSF project](https://osf.io/vbr7h/) is the designated archive for the deposited XTPrecip research materials, including the XTP11 training and validation data. Avoid placing large training arrays or checkpoint binaries directly in Git history. The precise contents and downloadable file versions can be viewed on the OSF page.

## 7. Research uses and outputs

XTPrecip supports research into multi-source precipitation estimation, spatiotemporal deep learning, predictor importance, regional transferability, model stochasticity, and precipitation modeling in gauge-sparse regions.

The code and notebooks support the experiments presented in the XTPrecip paper. The resulting **ML4HP-US** dataset, covering CONUS at nominal 4-km hourly resolution for 2002–2025, is documented separately and is planned for public release by the end of 2026.

## 8. Citation

If you use XTPrecip or reproduce the paper's experiments, please cite the associated XTPrecip research paper. Please also reference the [XTPrecip OSF project](https://osf.io/vbr7h/) when using files downloaded from that archive. If the OSF project provides a formal citation or DOI, use the citation shown on its project page.

**XTPrecip paper citation:** *[Full author list, article title, journal, year, DOI to be added after confirmation]*

Citations for third-party gridded input products and rain-gauge datasets should follow the corresponding data providers' guidance.

## 9. License and acknowledgments

The GitHub repository currently displays an **MIT License**. See [`LICENSE`](LICENSE) for the legal terms applicable to the repository software. Rights and redistribution conditions for **third-party input datasets, station observations, pretrained model weights, and large training arrays** should be checked separately; the software license does not automatically override their terms.

*Funding acknowledgments, computing-resource acknowledgments, contact details, exact dependency versions, and the full publication citation can be added once confirmed from the manuscript and repository files.*
