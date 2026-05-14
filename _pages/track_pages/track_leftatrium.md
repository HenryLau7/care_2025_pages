---
layout: distill
title: CARE-Left Atrium
description: Left Atrial Segmentation and Analysis
permalink: /track_leftatrium/
bibliography: reference.bib
toc:
  - name: Registration
  - name: Motivation
  - name: Task
  - name: Data
  - name: Metrics
  - name: Rules
  - name: Leaderboards
  - name: Citations
  - name: Contact
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
To access the dataset, please register [here](http://zmic.org.cn/care_2026/eval/register?track=leftatrium).


## Motivation

{% include figure.liquid loading="eager" path="/assets/img/lascarqs1.png" class="img-fluid" zoomable=true caption="Figure 1." %}
Atrial fibrillation (AF), the most prevalent cardiac arrhythmia, is poised to escalate in frequency due to aging populations <d-cite key="lascarqs1"></d-cite>.
Radiofrequency catheter ablation, a common AF therapy, faces challenges due to high recurrence rates <d-cite key="lascarqs2"></d-cite>.
Cardiac digital twin provides personalized *in-silico* cardiac representations to infer multi-scale properties associated with cardiac mechanisms <d-cite key="lascarqs3"></d-cite><d-cite key="lascarqs4"> </d-cite>. 
It has shown great promise in personalized targeted ablation of persistent AF <d-cite key="lascarqs5"></d-cite> (see Fig. 1).
To create a digital twin, it is important to reconstruct the left atrial (LA) geometry with the location of scars from LGE MRI <d-cite key="lascarqs6"></d-cite>.
However, automatic quantification and analysis of LA scars can be quite challenging due to the low image quality, thin wall, the surrounding enhanced regions, and the complex patterns of scars <d-cite key="lascarqs7"></d-cite>.
Deep learning (DL) methods have shown promise in LGE MRI analysis, yet their performance often falters in new domains due to domain shifts <d-cite key="lascarqs8"></d-cite><d-cite key="lascarqs9"></d-cite>.
CARE-Left Atrium aims to address these issues, driving the advancement of DL models that precisely delineate LA cavity and scars and ultimately revolutionize personalized AF treatment.

## Task

{% include figure.liquid loading="eager" path="/assets/img/lascarqs2.png" class="img-fluid" zoomable=true caption="Figure 2." %}
The target of this track is to automatically segment LA cavity and quantify LA scars from LGE MRI (see Fig. 2). The track will provide 500+ LGE MRIs and CTs globally, i.e., from multiple imaging centers around the world, for developing novel algorithms that can quantify or segment LA cavity and scars. The track presents an open and fair platform for various research groups to test and validate their methods on these datasets acquired from the clinical environment. To ensure data privacy, the platform will enable remote training and testing on the dataset from different centers in local and the dataset can keep invisible.

The selected papers will be published in our proceedings (see [previous proceedings](https://www.google.co.uk/books/edition/Left_Atrial_and_Scar_Quantification_and/dkq9EAAAQBAJ?hl=en&gbpv=0)).

Topics may cover (not exclusively):
- Cardiac digital twins
- Atrial fibrillation
- Cardiac image segmentation
- Model generalization
- Joint optimization
- Multi-task learning
- Personalized healthcare


## Data

### Data acquisition information
We include 200+ multi-center LGE MRIs and 300 CTs (enhanced.nii.gz) from different countries, with manual segmentation of LA cavity (atriumSegImgMO.nii.gz) and/ or scarring region (scarSegImgM.nii.gz).
All these clinical data have got institutional ethic approval and have been anonymized (please follow the data usage agreement, i.e., CC BY NC ND).
The details of these LGE MRI are listed below:

**Center A**: 154 LGE MRIs

This data was original collected from Utah [NAMIC-CARMA](https://www.insight-journal.org/midas/collection/view/197) with permission for release. [2018 Atrial Segmentation Challenge](https://atriaseg2018.cardiacatlas.org/) refined the LA segmentation of Utah NAMIC-CARMA dataset before final release.  Therefore, we adopted the refine dataset, and further fixed the resolution irregularities existing in this dataset. The clinical images were acquired with Siemens Avanto 1.5T or Vario 3T using free-breathing (FB) with navigator-gating.  The spatial resolution of the 3D LGE MRI scan was 0.625 × 0.625 × 2.5 mm.  The patient underwent an MR examination prior to ablation or was 3-6 months after ablation.

**Center B**: 20 LGE MRIs

This data was original collected from Beth Israel Deaconess Medical Center and was used in [ISBI2012 Left Atrium Fibrosis and Scar Segmentation Challenge](https://www.cardiacatlas.org/challenges/left-atrium-fibrosis-and-scar-segmentation-challenge/). We selected part of the dataset from this challenge and refine their manual segmentation before release. The clinical images were acquired with Philips Acheiva 1.5T using FB and navigator-gating with fat suppression. The spatial resolution of one 3D LGE MRI scan was 1.4 × 1.4 × 1.4 mm. The patient underwent an MR examination prior to ablation or was 1 month after ablation.

**Center C**: 20 LGE MRIs

This data was original collected from King’s College London and was used in [ISBI2012 Left Atrium Fibrosis and Scar Segmentation Challenge](https://www.cardiacatlas.org/challenges/left-atrium-fibrosis-and-scar-segmentation-challenge/). We selected part of the dataset from this challenge and refine their manual segmentation before release. The clinical images were also acquired with Philips Acheiva 1.5T using FB and navigator-gating with fat suppression. The spatial resolution of one 3D LGE MRI scan was 1.3 × 1.3 × 4.0 mm. The patient underwent an MR examination prior to ablation or was 3-6 months after ablation.

**Center D**: 300 CTs

This data was original collected from Fuzhou University Affiliated Provincial Hospital. We selected part of the dataset from this challenge and refine their manual segmentation before release.The clinical CT images were acquired with Siemens Force. The data were acquired at a resolution various from (0.30 x 0.30 x 0.5) to (0.80 x 0.80 x 0.5) mm.

<!--
*Center C-2*: 40 LGE MRIs

This data was collected from King’s College London/ St Thomas' Hospital with permission for release. All patients underwent CMR imaging on a 1.5T scanner (Magnetom Area, Siemens Healthineers, Erlangen, Germany) using a previously described protocol. Twenty minutes after contrast administration, late gadolinium enhancement imaging was performed using an ECG-triggered, respiratory navigated, 3D whole heart, inversion recovery spoiled gradient echo sequence in axial orientation (spatial resolution 1.3 mm × 1.3 mm × 4.0 mm reconstructed to 1.3 × 1.3 × 2 mm, TR 4 ms, TE 2 ms, flip angle 20°), phase encoding direction; anterior–posterior, frequency encoding direction; right–left, parallel imaging; GRAPPA factor 2.
-->


### Data split

The dataset has been divided into three main parts: training, validation, and test sets:

**Task 1: LA scar quantification (MRI)**:
- **Training Set**: 60 LGE MRIs from Center A 
- **Validation Set**: 10 LGE MRIs from Center A 
- **Test Set**: 24 LGE MRIs from Center A 
  
**Task 2: LA cavity segmentation (MRI)**:
- **Training Set**: 130 LGE MRIs from Centers A 
- **Validation Set**: 10 LGE MRIs from Center A and 10 LGE MRIs from Center C 
- **Test Set**: 14 LGE MRIs from Center A, 20 LGE MRIs from Center B and 10 LGE MRIs from Center C  <!-- , 40 LGE MRIs from Center 2.2-->

**Task 3: LA multi-structure segmentation (CT)**:
- **Training Set**: 150 CTs from Center D
- **Validation Set**: 20 CTs from Center D
- **Test Set**: 130 CTs from Center D 

### Data Format
Each LGE MRI, CT and gold standard label(s) of patients will be provided in the NIfTI format as follows:
- enhanced.nii.gz (LGE MRI)s'ba
- atriumSegImgMO.nii.gz (gold standard LA cavity label)
- scarSegImgM.nii.gz (gold standard LA scar label, for task 1 only)
- cardiacSegImgMO.nii.gz(gold standard labels of left atrium, pulmonary veins and left atrial appendage, for task 3 only)

The submitted format of the prediction for the participants could be named as follows:
- LA_predict.nii.gz (predicted LA cavity label)
- cardiac_predict.nii.gz (predicted cardiac label)
- scar_predict.nii.gz (predicted LA scar label)


## Metrics

The performance of LA cavity segmentation, LA scar quantification and cardiac structure segmentation results will be evaluated by：

**Task 1: LA scar quantification (MRI)**:
- **Generalized Dice Similarity Coefficient (G-DSC)** <d-cite key="lascarqs6">
- **Accuracy (ACC)**
- **Sensitivity (SEN)**

**Task 2: LA cavity segmentation (MRI)**:
- **Dice Similarity Coefficient (DSC)**
- **Hausdorff Distance (HD)**

**Task 3: LA multi-structure segmentation (CT)**:
- **Dice Similarity Coefficient (DSC)**
- **Hausdorff Distance (HD)**

## Rules
1. External data sets and pre-trained models are allowed in this track.
2. Only automatic methods are permitted.
3. Participants are encouraged to attempt all tasks, but they also can choose to focus on one of them. 
4. We will only award prizes for LA scar quantification (MRI) and LA multi-structure segmentation (CT).

## Leaderboards
Leaderboards will be released after test results submission.

## Citations
**Please cite these papers when you use the data for publications:**
```bib
 @article{zhuang2019multivariate,
    title={Multivariate mixture model for myocardial segmentation combining multi-source images},
    author={Zhuang, Xiahai},
    journal={IEEE transactions on pattern analysis and machine intelligence},
    volume={41},
    number={12},
    pages={2933--2946},
    year={2019},
}

@article{zhuang2016multi,
  title={Multi-scale patch and multi-modality atlases for whole heart segmentation of MRI},
  author={Zhuang, Xiahai and Shen, Juan},
  journal={Medical image analysis},
  volume={31},
  pages={77--87},
  year={2016},
  publisher={Elsevier}
}

@article{li2022atrialjsqnet,
  title={AtrialJSQnet: a new framework for joint segmentation and quantification of left atrium and scars incorporating spatial and shape information},
  author={Li, Lei and Zimmer, Veronika A and Schnabel, Julia A and Zhuang, Xiahai},
  journal={Medical image analysis},
  volume={76},
  pages={102303},
  year={2022},
  publisher={Elsevier}
}

@article{GAO2023BayeSeg,
  title = {BayeSeg: Bayesian modeling for medical image segmentation with interpretable generalizability},
  journal = {Medical Image Analysis},
  volume = {89},
  pages = {102889},
  year = {2023},
  author = {Shangqi Gao and Hangqi Zhou and Yibo Gao and Xiahai Zhuang},
}
```


<script defer src="{{ '/assets/js/leaderboard_sort.js' | relative_url }}"></script>

## Contact
If you have any questions regarding the CARE-Whole Heart track, please feel free to contact:
[care26challenge@163.com](mailto:care26challenge@163.com) or [care2026challenge@outlook.com](mailto:care2026challenge@outlook.com).
