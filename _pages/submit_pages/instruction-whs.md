---
layout: page
permalink: /instruction_whole_heart/
---

# CARE 2026 – Whole Heart Track Test Phase Docker Submission Instructions

Thank you for participating in the CARE 2026 Whole Heart Track!

During the **test phase**, each team must submit a **Docker image** containing their trained algorithm. The organizers will evaluate this image on a hidden test set to ensure fairness and reproducibility.

## Introduction

Prepare a Docker image with your prediction pipeline and submit the following via email:

- **Email address**: [care26challenge@163.com]() or [care2026challenge@outlook.com]()
- **Email subject format**: [CARE-Whole Heart Test] `Team-Name` – Docker Submission

Include in the email body:
- A **download link** to your Docker image, hosted on Google Drive, Mega, Baidu Netdisk, or a similar service.
- The **docker commands** to run the Docker container (if not using an ENTRYPOINT), along with any additional instructions required to execute your pipeline. Note: we would like to run all code **CPU-only** for compatibility reasons. If you want to use a GPU, please let us know.

Each team is allowed **up to two submissions**. Failed submissions (e.g., due to runtime errors or invalid outputs) **do not count** toward this limit. After a successful first submission, we will provide a feedback report with evaluation metrics. You may then submit an improved Docker image for the second attempt.

## Docker Requirements

Your Docker container must:
- **Read input images** from the mounted `/input` directory (read-only).
- **Write predictions** to the mounted `/output` directory.

We will load and run your Docker image using commands similar to the following:

```bash
docker load --input <your_image_file>

docker run \
    −v <our_test_directory>:/input:ro \
    −v :/output \
    −it <your_image_file> <your_command>
```

- The container should run in a non-interactive mode if possible; avoid requiring user input.
- Ensure the container exits gracefully upon completion.

Note that the file strucure of /input is as follows:
```
input/
│    ├──ct_test/
│    │    ├──CaseCTTest001_image.nii.gz
│    │    ├──CaseCTTest002_image.nii.gz
│    │    ├──CaseCTTest003_image.nii.gz
│    │    ***
│    ├──mr_test/
│    │    ├──CaseMRTest001_image.nii.gz
│    │    ├──CaseMRTest002_image.nii.gz
│    │    ├──CaseMRTest003_image.nii.gz
│    │    ***
```

The expected output structure mirrors the validation phase：
```
output/
│    ├──ct_test/
│    │    ├──CaseCTTest001_pred.nii.gz
│    │    ├──CaseCTTest002_pred.nii.gz
│    │    ├──CaseCTTest003_pred.nii.gz
│    │    ***
│    ├──mr_test/
│    │    ├──CaseMRTest001_pred.nii.gz
│    │    ├──CaseMRTest002_pred.nii.gz
│    │    ├──CaseMRTest003_pred.nii.gz
│    │    ***

```

We will copy the prediction results in /output to our local directory and do evaluation.


## Building Your Docker Image

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

You can view [Docker Homepage](www.docker.com) for further reference.


### Additional Guidelines
- **Predictions**: Must match the input images' shapes, orientations, and data types.
- **No Training**: The container should only perform inference; no training or model updates are allowed.
- **No Internet Access**: The container must run offline—do not include dependencies requiring network access (e.g., no pip installs at runtime).
- **Efficiency**: Optimize for reasonable runtime on standard hardware (e.g., GPU if specified; indicate in your instructions if a GPU is required via `--gpus all`).
- **Error Handling**: Include robust logging and error handling to aid debugging.


## Important Dates
- **Test Phase Start**: June 10, 2026
- **Docker Submission Deadline**: July 10, 2026
- **Abstract & Paper Submission Deadline**: July 20, 2026


## Final Notes
- The validation phase remains active during the test phase; you may continue submitting validation predictions as per previous instructions, and the leaderboard will update accordingly.
- For questions or issues, contact: [care26challenge@163.com]() or [care2026challenge@outlook.com]()