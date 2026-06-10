---
layout: page
permalink: /instruction_myocardium/
---

# CARE 2026 – Myocardium Track Test Phase Docker Submission Instructions

Thank you for participating in the CARE 2026 Myocardium Track!

During the **test phase**, each team must submit a **Docker image** containing their trained algorithm. The organizers will evaluate this image on a hidden test set to ensure fairness and reproducibility.

## Submission Overview

Prepare a Docker image with your prediction pipeline and submit the following via email:

- **Email address**: [care26challenge@163.com]() or [care2026challenge@outlook.com]() 
- **Email subject format**: [CARE-Myocardium Test] `Team-Name` – Docker Submission

Include in the email body:
- A **download link** to your Docker image (in `.tar.gz` or `.tar` format), hosted on Google Drive, Mega, Baidu Netdisk, or a similar service.
- The **command** to run the Docker container (if not using an ENTRYPOINT), along with any additional instructions required to execute your pipeline. Note: we would like to run all code **CPU-only** for compatibility reasons. If you really want to use a GPU, please let us know.
- The specific **task** this Docker image is for (submit only for the tasks you are participating in):
  - `MyoPS`
  - `CineMyoPS`

Each team is allowed **up to two submissions per task**. Failed submissions (e.g., due to runtime errors or invalid outputs) **do not count** toward this limit. After a successful first submission, we will provide a feedback report with evaluation metrics. You may then submit an improved Docker image for the second attempt.

## Running Your Docker

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

Note that the file structure of /input is as follows:
```
        :/input
        ├── myops/
        │   ├── Case1021_C0.nii.gz
        │   ├── Case1021_LGE.nii.gz
        │   └── Case1021_T2.nii.gz
        │       ...
        └── cinemyops/
            └── Case1021_Cine.nii.gz
                ...
```

The prediction results we hope to obtain is as follows：

- For MyoPS task: Predictions in `/output/`, structured as:
```
        ├── myops/
        │   └── Case1021_pred.nii.gz
        │       ...
```

- For CineMyoPS task: Predictions in `/output/`, structured as:
```
        ├── cinemyops/
        │   └── Case1021_pred.nii.gz
        │       ...

```
We will copy the prediction results in /output to our local directory and do evaluation.


## Building Your Docker

This section specifies some basic methodology on building docker from your original scripts by a python example based on [hjkuijf/wmhchallenge](https://github.com/hjkuijf/wmhchallenge).

Let `[cwd]` stands for your current working directory, and your python source codes are stored in `[cwd]/python/src`.

To build a docker based on your python codes, you can create a *Dockerfile* at `[cwd]`, a valid template can be like:

```
FROM continuumio/miniconda
MAINTAINER hjkuijf

RUN pip install numpy

ADD python/src /CMT
```

where the first two lines tell docker to inherit a base image from [docker hub](hub.docker.com) that setups a python environment, the third line tells docker to install numpy package, and the last line tells docker to copy your python code files into the container directory `/CMT.`

For further reference about editing *Dockerfile*, you can visit [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/).

Once the source codes and the dockerfile are ready, you can use `docker build` to set up your container.

```
docker build -f Dockerfile -t [your_docker_name] .
```

And your docker command should be

```
python /CMT/your_code.py
```

You can use "docker save" command to export your docker image into a tar file. We also suggest that you wrap the file into a gzip package.

```
docker save <your_image> -o <output_file_name>
docker save <your_image> | gzip > <output_gzip_file_name>
```

You can use any of the public testing image volumes to check whether your Docker image runs as expected.

You can view [Docker Homepage](www.docker.com) for further reference.


## Naming Convention

To facilitate organization, name your Docker image files as follows:

```
MyoPS-<TeamName>.tar.gz
CineMyoPS-<TeamName>.tar.gz
```

## Important Dates
- **Test Phase Start**: June 10, 2026
- **Docker Submission Deadline**: July 10, 2026
- **Abstract & Paper Submission Deadline**: July 20, 2026


## Final Notes
- Submit separate Docker images for each task if participating in multiple.
- Ensure output names and formats are consistent with validation phase submissions.
- The validation phase remains active during the test phase; you may continue submitting validation predictions as per previous instructions, and the leaderboard will update accordingly.
- For questions or issues, contact: [care26challenge@163.com]() or [care2026challenge@outlook.com]()
