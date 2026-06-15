---
layout: distill
title: CARE-Liver
description: Liver Fibrosis Quantification and Analysis
permalink: /track_liver/
bibliography: reference.bib
toc:
  - name: Registration
  - name: Motivation
  - name: Tasks
  - name: Data
  - name: Rules
  - name: Leaderboards
  - name: Citations
_styles: >
  d-article {
    contain: layout style;
    overflow-x: hidden;
    border-top: 1px solid rgba(0, 0, 0, 0.1);
    padding-top: 2rem;
    color: rgba(0, 0, 0, 0.8);
  }

  d-article > * {
    grid-column: text;
  }
---
## Registration
To access the dataset, please register [here](http://zmic.org.cn/care_2026/eval/register?track=liver).

## Motivation
{% include figure.liquid loading="eager" path="/assets/img/liqa1.png" class="img-fluid" zoomable=true caption="Figure 1. Track description." max-width="70%" %}

Liver fibrosis, arising from chronic viral or metabolic liver conditions, presents a significant global health challenge. Precise liver segmentation (LiSeg) and fibrosis staging (LiFS) are critical for enabling precise disease management, prognostication, and informed clinical decision-making. This challenge focuses on advancing **two interconnected objectives** to bridge clinical needs with AI innovation:  
1. **Automated Liver Segmentation**.  
2. **Multi-Phase Fibrosis Staging**.

Participants will develop robust AI solutions using multi-center, multi-phase MRI data, addressing real-world variability in imaging protocols and scanner systems.  

- **Recommended reading: Participants are encouraged to refer to our recent LiFS benchmark paper, ["How Far Has AI Come in Liver Fibrosis Staging? A Large-Scale Real-World Dataset and Benchmark"](https://arxiv.org/abs/2605.25595). This benchmark systematically evaluated MRI-based AI methods for liver fibrosis staging, including participating methods from CARE2025, under realistic multi-center settings. It also discusses challenges that are directly relevant to this track, including cross-center generalization, domain shift, contrast-enhanced imaging, label imbalance, and clinically meaningful evaluation endpoints.**

## Tasks
### **Task 1: Liver Fibrosis Staging (LiFS)**  
Develop models to stage fibrosis into four stages (S1-S4), leveraging **cross-phase complementary information** from dynamic MRI sequences. Participants should submit one four-class probability vector (S1-S4) per case. The organisers will derive two clinically critical binary evaluations from this single output, so separate models for the two clinical tasks are not required:  
1. **Cirrhosis Detection**: S1–S3 vs. S4  
2. **Substantial Fibrosis Detection**: S1 vs. S2–S4  

#### Subtasks:  
- **Non-Contrast Subtask**: T1WI, T2WI, and DWI sequences only. Submissions that do not use any GED modality are evaluated in this subtask.  
- **Contrast-Enhanced Subtask**: All modalities are allowed, including Gd-EOB-DTPA phases (GED1–GED4). Any method that uses at least one GED modality is evaluated in this subtask.  

### **Task 2: Liver Segmentation (LiSeg)**  
Segment the liver in multi-phase fibrosis, where **limited ground truth of Hepatobiliary phase (GED4) MRI** is available. Non-contrast data (T2WI, T1, DWI) could be segmented via **unsupervised or registration-based approaches** to overcome annotation limitations. For evaluation, metrics are reported for submitted modalities only (for example, GED4-only submissions receive GED4 metrics only).  

#### Subtasks:
- **Non-Contrast Subtask**: Segment liver anatomy in **T2WI**, **T1**, and **DWI** sequences. No annotations provided.
- **Contrast-Enhanced Subtask**: Segment **GED4** sequence. Limited annotations provided.

---

## Data

### About the data

**1) Scanner:** Philips Ingenia3.0T, Siemens Skyra 3.0T, Siemens Aera 1.5T.

**2) Dataset overview:**  The track cohort comprises **800 patients** (190 newly cases compared to CARE2025) diagnosed with liver fibrosis, all of whom underwent multi-phase MRI scans. The dataset includes **multi-phase** and **multi-center** data, with images acquired from clinical centers using three different MRI scanner vendors. The dataset consists of T2-weighted imaging, diffusion-weighted imaging, and Gadolinium ethoxybenzyl diethylenetriamine pentaacetic acid (**Gd-EOB-DTPA**)-enhanced dynamic MRIs. The Gd-EOB-DTPA-enhanced dynamic MRIs cover the non-contrast phase (T1WI), arterial phase, venous phase, delayed phase, and hepatobiliary phase.

**3) Contrast-enhanced dynamic scans:** Contrast-enhanced scans were performed based on the injection of the GD-EOB-DTPA agent. The arterial phase is captured 25 seconds after the contrast agent is injected. Subsequently, the portal phase is achieved 1 minute later. After another 3 minutes, the delay phase is obtained, and finally, the hepatobiliary phase is reached 20 minutes thereafter.

**4) Data format:** The data are all in Nifty format. Each sample may randomly lack phases (except hepatobiliary phase), and the sequences have not applied pre-alignment through spatial registration.

### Training set

<div style="display: flex;">
<table class="table table-sm table-hover border-bottom" style="table-layout:fixed;width:80%;align:center;">
  <thead>
    <tr>
      <th class="text-center" style="text-align:center;">Vendor</th>
      <th class="text-center" style="text-align:center;">Center</th>
      <th class="text-center" style="text-align:center;">#Cases</th>
      <th class="text-center" style="text-align:center;">#Annotation for seg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="text-center">A</td>
      <td class="text-center">A</td>
      <td class="text-center">157</td>
      <td class="text-center">10</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B1</td>
      <td class="text-center">230</td>
      <td class="text-center">10</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B2</td>
      <td class="text-center">73</td>
      <td class="text-center">10</td>
    </tr>
  </tbody>
</table>
</div>


### Validation Set

<div style="display: flex;">
<table class="table table-sm table-hover border-bottom" style="table-layout:fixed;width:80%;align:center;">
  <thead>
    <tr>
      <th class="text-center" style="text-align:center;">Vendor</th>
      <th class="text-center" style="text-align:center;">Center</th>
      <th class="text-center" style="text-align:center;">#Cases</th>
      <th class="text-center" style="text-align:center;">#Annotation for seg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="text-center">A</td>
      <td class="text-center">A</td>
      <td class="text-center">20</td>
      <td class="text-center">20</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B1</td>
      <td class="text-center">20</td>
      <td class="text-center">20</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B2</td>
      <td class="text-center">20</td>
      <td class="text-center">20</td>
    </tr>
  </tbody>
</table>
</div>

### Test Set

<div style="display: flex;">
<table class="table table-sm table-hover border-bottom" style="table-layout:fixed;width:50%;align:center;">
  <thead>
    <tr>
      <th class="text-center" style="text-align:center;">Vendor</th>
      <th class="text-center" style="text-align:center;">Center</th>
      <th class="text-center" style="text-align:center;">#Cases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="text-center">A</td>
      <td class="text-center">A</td>
      <td class="text-center">50</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B1</td>
      <td class="text-center">50</td>
    </tr>
    <tr>
      <td class="text-center">B</td>
      <td class="text-center">B2</td>
      <td class="text-center">50</td>
    </tr>
    <tr>
      <td class="text-center">C</td>
      <td class="text-center">C</td>
      <td class="text-center">130</td>
    </tr>
  </tbody>
</table>
</div>

- **NOTE: The evaluation on the unseen dataset (C) highlights the generalization capabilities of the models and algorithms, which we particularly emphasize.**

## Rules
1. Publicly available data (such as [LLD-MMRI2023](https://github.com/LMMMEng/LLD-MMRI2023)) and pre-trained models are allowed. 
2. Only automatic methods are acceptable. 

## Leaderboards
For LiFS, participants submit four-class probabilities for fibrosis staging (S1-S4). The organisers will compute AUC and ACC for two derived binary clinical subtasks: S1–S3 vs. S4 and S1 vs. S2–S4. Methods that use any contrast-enhanced GED modality are evaluated under the contrast-enhanced subtask, while methods that use only non-contrast modalities are evaluated under the non-contrast subtask.

For LiSeg, participants are required to segment the liver across multi-phase fibrosis MRI. We report Dice Similarity Score (DSC) and Hausdorff Distance (HD) only for the modalities included in each submission. If only GED4 is submitted, only GED4 metrics are reported; if additional modalities are submitted, their corresponding metrics are also reported.

The evaluation metrics for each task are summarized in the table below. The leaderboard will rank all participating teams according to the key metrics of each subtask, including AUC and ACC for LiFS.


<div style="display: flex;">
<table class="table table-sm table-hover border-bottom" style="table-layout:fixed;width:80%;align:center;">
  <thead>
    <tr>
      <th style="text-align:center;">Task</th>
      <th style="text-align:center;">Subtask</th>
      <th style="text-align:center;">Key Metrics</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2"><strong>LiFS</strong></td>
      <td>Non-Contrast</td>
      <td>AUC, ACC (S4 vs. S1–S3; S1 vs. S2–S4)</td>
    </tr>
    <tr>
      <td>Contrast-Enhanced</td>
      <td>AUC, ACC (S4 vs. S1–S3; S1 vs. S2–S4)</td>
    </tr>
    <tr>
      <td rowspan="2"><strong>LiSeg</strong></td>
      <td>Non-Contrast (T1W1/T2WI/DWI)</td>
      <td>Dice Similarity Score (Dice), Hausdorff Distance (HD in mm)</td>
    </tr>
    <tr>
      <td>Contrast-Enhanced (GED4)</td>
      <td>Dice Similarity Score (Dice), Hausdorff Distance (HD in mm)</td>
    </tr>
  </tbody>
</table>
</div>

- **NOTE: Participants are welcome to participate in a single subtask, and their performance will still be recorded and displayed on the leaderboard. For LiSeg, scores are reported only for submitted modalities.**


## Citations
**Please cite these papers when you use the data for publications:**
```bib

@misc{liu2026lifs,
      title={How Far Has AI Come in Liver Fibrosis Staging? A Large-Scale Real-World Dataset and Benchmark}, 
      author={Yuanye Liu and Nannan Shi and Zhejia Zhang and Hanxiao Zhang and Boya Wang and Derong Yu and Nao Wang and Yuxin Jin and Yang Zhou and Kunhao Yuan and Siqi Wang and Lida Yang and Xu Qiao and Wentao Liu and Xuelei He and Xin Hong and Guoyan Zheng and Xin Chen and Guang-Zhong Yang and Le Zhang and Lei Li and Yuxin Shi and Xiahai Zhuang},
      year={2026},
      eprint={2605.25595},
      archivePrefix={arXiv},
      url={https://arxiv.org/abs/2605.25595}, 
}

@article{liu2025merit,
  title = {MERIT: Multi-view evidential learning for reliable and interpretable liver fibrosis staging},
  author={Liu, Yuanye and Gao, Zheyao and Shi, Nannan and Wu, Fuping and Shi, Yuxin and Chen, Qingchao and Zhuang, Xiahai},
  journal = {Medical Image Analysis},
  volume = {102},
  pages = {103507},
  year = {2025}
}

@inproceedings{gao2023reliable,
  title={A reliable and interpretable framework of multi-view learning for liver fibrosis staging},
  author={Gao, Zheyao and Liu, Yuanye and Wu, Fuping and Shi, Nannan and Shi, Yuxin and Zhuang, Xiahai},
  booktitle={International Conference on Medical Image Computing and Computer-Assisted Intervention},
  pages={178--188},
  year={2023}
}

@article{wu2022meru,
  title = {Minimizing Estimated Risks on Unlabeled Data: A New Formulation for Semi-Supervised Medical Image Segmentation},
  author={Wu, Fuping and Zhuang, Xiahai},
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence}, 
  title={Minimizing Estimated Risks on Unlabeled Data: A New Formulation for Semi-Supervised Medical Image Segmentation}, 
  year={2023},
  volume={45},
  number={5},
  pages={6021-6036},
}
```

<script defer src="{{ '/assets/js/leaderboard_sort.js' | relative_url }}"></script>
