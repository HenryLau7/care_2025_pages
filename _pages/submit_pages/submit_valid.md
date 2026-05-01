---
layout: page
title: Validation Submission
permalink: /valid_submission/
---

Participants can directly upload your predictions (in nifty format) via the evaluation platforms. Note that evaluation of validation data will be allowed up to **10** times for each task per team.

#### Summary of Evaluation Platforms

- [**CARE-Left Atrium** evaluation platform](https://zmic.org.cn/care_2026/eval/login?track=leftatrium)
- [**CARE-Liver** evaluation platform](https://zmic.org.cn/care_2026/eval/login?track=liver)
- [**CARE-Myocardium** evaluation platform](https://zmic.org.cn/care_2026/eval/login?track=myocardium)
- [**CARE-Whole Heart** evaluation platform](https://zmic.org.cn/care_2026/eval/login?track=wholeheart)
<<<<<<< HEAD


Please organize your files as follows:

- **CARE-Left Atrium**:
```
CARE-Leftatrium-TeamName.zip
├── LA scar quantification
│  ├── val_****
│   │  ├──****_pred.nii.gz       #Only need to predict scar
├── LA cavity segmentation
│  ├── val_****
│   │  ├──****_pred.nii.gz
├── LA multi-structure segmentation
│  ├── val_****
│   │  ├──****_pred.nii.gz
```

- **CARE-Liver**:
```
CARE-Liver-Submission.zip/
├── LiFS_pred.csv
├── LiSeg_pred/
│   ├── 0001-A/
│   │   ├── GED4_pred.nii.gz
│   │   ├── DWI_800_pred.nii.gz
│   │   └── T2_pred.nii.gz
│   └── ...
```

- **CARE-Myocardium**:
```
CARE-Myocardium-TeamName.zip
├── MyoPS
│   ├── Anonymous Center 
│   │   ├── Case****
│   │   │   ├──Case****_pred.nii.gz
├── CineMyoPS
│   ├── Anonymous Center 
│   │   ├── Case****
│   │   │   ├──Case****_pred.nii.gz
```

- **CARE-Whole Heart**:
```
CARE-Wholeheart-TeamName.zip
│    ├──ct_val
│    │    ├──CaseCTVal001_pred.nii.gz
│    │    ├──CaseCTVal002_pred.nii.gz
│    │    ├──CaseCTVal***_pred.nii.gz
│    │    ***
│    ├──mr_val
│    │    ├──CaseMRVal001_pred.nii.gz
│    │    ├──CaseMRVal002_pred.nii.gz
│    │    ├──CaseMRVal***_pred.nii.gz
│    │    ***
```
=======
>>>>>>> refs/remotes/origin/main
