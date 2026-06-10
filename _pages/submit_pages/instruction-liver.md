---
layout: page
permalink: /instruction_liver/
---

# CARE 2026 – Liver Track Test Phase Docker Submission Instructions

Thank you for participating in the CARE 2026 Liver Challenge!

> **NOTE:** the Contrast / NonContrast sub-tracks have been removed. There are now two tasks only — **LiFS** and **LiSeg** — and for LiFS you must **declare which input modalities you used** for each case (see the output format below).

During the **test phase**, each team must submit a **Docker image** containing their trained algorithm. The organizers will evaluate this image on a hidden test set to ensure fairness and reproducibility.

## Submission Overview

Prepare a Docker image with your prediction pipeline and submit the following via email:

- **Email address**: [care26challenge@163.com]() or [care2026challenge@outlook.com]()  
- **Email subject format**: [CARE-Liver Test] `Team-Name` – Docker Submission

Include in the email body:
- A **download link** to your Docker image (in `.tar.gz` or `.tar` format), hosted on Google Drive, Mega, Baidu Netdisk, or a similar service.
- The **command** to run the Docker container (if not using an ENTRYPOINT), along with any additional instructions required to execute your pipeline.
- The specific **task** this Docker image is for (submit only for the tasks you are participating in):
  - `LiFS` — Liver Fibrosis Staging
  - `LiSeg` — Liver Segmentation
- **For `LiFS` submissions — the exact list of input modalities your pipeline requires** (e.g., `DWI_800, T1, T2`). Based on this list, we will prepare the test input dataset so that **only the modalities you specify are placed in `/input`** — any modality you do not list will not be provided. Please state this clearly in your email. (For `LiSeg`, all available modalities are provided by default.)

Each team is allowed **up to two successful submissions per task (LiFS / LiSeg)**. Failed submissions (e.g., due to runtime errors or invalid outputs) **do not count** toward this limit. After a successful first submission, we will provide a feedback report with evaluation metrics. You may then submit an improved Docker image for the second attempt.

## Test Data Format

The test set follows the **same format as the validation set**, with data from four vendors: **A, B1, B2, C**. The directory structure is as follows:

```
Liver-Test-Dataset/
├── info.csv
├── Vendor-A/
│   └── XXXX-X/
│       ├── DWI_800.nii.gz
│       ├── GED1.nii.gz
│       ├── GED2.nii.gz
│       ├── GED3.nii.gz
│       ├── GED4.nii.gz
│       ├── T1.nii.gz
│       ├── T2.nii.gz
├── Vendor-B1/
├── Vendor-B2/
├── Vendor-C/
```

- All images are in `.nii.gz` format.

### Input Modalities

There are up to **7 modalities** in total: `DWI_800`, `GED1`, `GED2`, `GED3`, `GED4`, `T1`, `T2`. Your pipeline may use any subset of these; you are not required to use all of them. For **LiFS**, only the modalities you declare in your submission email will be placed in `/input` (see Submission Overview above).

- **Some cases may have missing modalities.** Even among the modalities you request, individual cases may lack one or more of them. Your pipeline must **autonomously detect missing modalities at runtime and handle them on its own** (e.g., skip, impute, or fall back), and still produce valid outputs for every case — the evaluation will not exclude such cases.
- To assist this, an `info.csv` file is provided in the dataset root and is **accessible to your container** (mounted at `/input/info.csv`), with the following columns:
  - `Case`: e.g., `0001-A`
  - `Vendor`: e.g., `A`
  - `Missing Modality`: e.g., `DWI_800` (blank if none)

## Docker Requirements

### Input/Output Mounts

Your Docker container must:
- **Read input images** from the mounted `/input` directory (read-only).
- **Write predictions** to the mounted `/output` directory.

We will load and run your Docker image using commands similar to the following:

```bash
docker load -i your_docker_image.tar.gz

docker run \
  -v /path/to/Liver-Test-Dataset:/input:ro \
  -v /path/to/output_dir:/output \
  -it your_image_name [your_command_if_needed]
```

- The container should run in a non-interactive mode if possible; avoid requiring user input.
- Ensure the container exits gracefully upon completion.

The expected output structure mirrors the validation phase:

- For the **LiFS** task: A single `LiFS_pred.csv` file in `/output`. LiFS is a **four-class** fibrosis staging task (S1–S4); provide the per-class probabilities and **declare the input modalities used** for each case in the `Modality Input` column:
```
Case,Modality Input,S1,S2,S3,S4
XXXX-X,"DWI, T1, T2",0.1,0.4,0.2,0.3
XXXX-X,"DWI, T1, T2, GED1, GED2, GED3, GED4",0.3,0.4,0.1,0.2
...
```
- For the **LiSeg** task: Predictions in `/output/LiSeg_pred/`, with one predicted file per segmented modality:
```
0001-A/
├── GED4_pred.nii.gz         # required
├── DWI_800_pred.nii.gz      # optional
├── T2_pred.nii.gz           # optional
...
```

### Special Note on Cascaded Pipelines
If your pipeline involves running segmentation first and then performing fibrosis staging based on the segmentation output (e.g., foreground liver regions), or other similar required cascaded pipelines, please describe this clearly in your instructions and emails. We will need to:
* First evaluate your segmentation task and store the outputs accordingly.
*	Then mount your segmentation predictions as accessible input to your fibrosis staging module.

### Naming Convention

To facilitate organization, name your Docker image files as follows:

```
LiFS-<TeamName>.tar.gz
LiSeg-<TeamName>.tar.gz
```

### Example: Building Your Docker Image

Here is a minimal Dockerfile example for a Python-based pipeline:

```dockerfile
FROM python:3.9-slim

WORKDIR /workspace

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENTRYPOINT ["python", "main.py"]
```

To build and export the image:

```bash
docker build -t your_image_name .
docker save your_image_name | gzip > your_image_name.tar.gz
```

- Replace `main.py` with your entry script, which should handle reading from `/input`, processing all cases, and writing to `/output`.
- Test your image locally by mounting sample directories to simulate the evaluation environment.

### Additional Guidelines
- **Predictions**: Must match the input images' shapes, orientations, and data types.
- **No Training**: The container should only perform inference; no training or model updates are allowed.
- **No Internet Access**: The container must run offline—do not include dependencies requiring network access (e.g., no pip installs at runtime).
- **Efficiency**: Optimize for reasonable runtime on standard hardware (e.g., GPU if specified; indicate in your instructions if a GPU is required via `--gpus all`).
- **Error Handling**: Include robust logging and error handling to aid debugging.

## Evaluation Details

Predictions will be evaluated offline by the organizers and the results published on the test leaderboard:
- **LiFS** (four-class staging S1–S4): Accuracy and AUC.
- **LiSeg**: Dice coefficient (Dice) and Hausdorff distance (HD), computed per segmented modality.

## Final Notes
- Submit separate Docker images for each task if participating in multiple.
- Ensure output names and formats are consistent with validation phase submissions.
- The validation phase remains active during the test phase; you may continue submitting validation predictions as per previous instructions, and the leaderboard will update accordingly.
- For questions or issues, contact: [care26challenge@163.com]() or [care2026challenge@outlook.com]()

## Important Dates
- **Test Phase Start**: June 10, 2026
- **Docker Submission Deadline**: July 10, 2026
- **Abstract & Paper Submission Deadline**: July 20, 2026