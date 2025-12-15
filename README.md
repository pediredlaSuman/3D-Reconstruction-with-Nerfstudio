# 3D-Reconstruction-with-Nerfstudio


This repository contains an **end-to-end 3D reconstruction pipeline** implemented using **Nerfstudio**. The project demonstrates how to reconstruct a 3D scene from a **single monocular video** using modern NeRF-based methods, covering environment setup, pose estimation, NeRF training, visualization, and point cloud export.

---

## 📌 Project Overview

The goal of this project is to reconstruct a high-quality 3D representation of a real-world scene from a simple 2D video without using specialized hardware such as LiDAR or multi-camera rigs. The pipeline leverages:

* **COLMAP** for camera pose estimation
* **Nerfstudio** for NeRF training (Nerfacto)
* **Docker + NVIDIA GPU acceleration** for environment stability
* **Point cloud export** for external 3D visualization

---

## 🧩 Features

* Full video-to-NeRF reconstruction pipeline
* Docker-based GPU-enabled environment
* Robust Nerfacto NeRF training
* Camera pose visualization
* Dense point cloud export (.ply)
* Reproducible and platform-independent setup

---

## 🏗 Repository Structure

```bash
3D-Reconstruction-Nerfstudio/
│
├── README.md                     # Project documentation
├── report/
│   └── Pediredla_Suman_3D_Nerfstudio_Report.pdf
│
├── scripts/
│   └── camera_pose_display.py    # Pose visualization script
│
├── docker/
│   └── docker_commands.sh        # Docker setup & run commands
│
├── sample_commands/
│   ├── video_processing.sh
│   ├── train_nerf.sh
│   └── export_pointcloud.sh
│
└── outputs/                      # (Optional) Example outputs / screenshots
```

---

## ⚙️ Environment Setup

### Requirements

* NVIDIA GPU (CUDA supported)
* Docker Desktop
* NVIDIA Container Toolkit

> ⚠️ Nerfstudio is **not officially supported on native Windows**. Docker is strongly recommended.

---

## 🐳 Docker Setup

### Pull Nerfstudio Docker Image

```bash
docker pull ghcr.io/nerfstudio-project/nerfstudio:latest
```

### Run Docker Container

```bash
docker run --gpus all -it \
-v C:\Users\<your_user>\nerf_video:/workspace/video \
-v C:\Users\<your_user>\nerf_output:/workspace/output \
-v C:\Users\<your_user>\nerf_train:/workspace/train \
-v C:\Users\<your_user>\nerf_exports:/workspace/exports \
-p 7007:7007 \
--rm --shm-size=12gb \
ghcr.io/nerfstudio-project/nerfstudio:latest
```

---

## 🎥 Video Processing & Pose Estimation

Convert a monocular video into a Nerfstudio-compatible dataset using COLMAP.

```bash
ns-process-data video \
--data /workspace/video/Sample_video.mov \
--output-dir /workspace/output \
--num-frames-target 100
```

### Output Generated

* Extracted image frames
* Estimated camera poses
* COLMAP sparse reconstruction

---

## 📐 Pose Visualization

Camera pose visualization helps validate reconstruction feasibility.

```bash
python camera_pose_display.py --input_dir /workspace/output
```

Visualizations include:

* 3D camera trajectory
* Top-down (bird’s-eye) view

---

## 🧠 NeRF Training (Nerfacto)

Train the NeRF model using Nerfstudio’s **Nerfacto** pipeline.

```bash
ns-train nerfacto \
--viewer.quit-on-train-completion True \
--pipeline.model.predict-normals True \
--vis viewer \
--data /workspace/output \
--output-dir /workspace/train
```

### Live Viewer

Access the interactive viewer at:

```
http://localhost:7007
```

---

## ☁️ Point Cloud Export

Export the trained NeRF model into a dense 3D point cloud.

```bash
ns-export pointcloud \
--load-config /workspace/train/output/nerfacto/<timestamp>/config.yml \
--output-dir /workspace/exports \
--num-points 1000000 \
--remove-outliers True \
--normal-method open3d \
--save-world-frame False
```

### Output

* `point_cloud.ply`
* Compatible with MeshLab, CloudCompare, Blender

---

## 🚧 Challenges & Solutions

| Challenge            | Solution                            |
| -------------------- | ----------------------------------- |
| Windows CUDA issues  | Docker-based deployment             |
| Dependency conflicts | Prebuilt Nerfstudio container       |
| Low pose matching    | Nerfacto’s robust training          |
| GPU memory limits    | Moderate resolution & viewer tuning |
| No wandb support     | Nerfstudio viewer monitoring        |

---

## 📊 Final Outputs

* Processed Nerfstudio dataset
* Camera pose visualizations
* Trained NeRF model checkpoints
* Interactive 3D viewer results
* Dense exported point cloud (.ply)

---

## ✅ Conclusion

This project demonstrates a **complete and reproducible 3D reconstruction pipeline** using Nerfstudio—from raw monocular video to an explicit 3D point cloud. By leveraging Docker and the Nerfacto pipeline, stable and high-quality reconstructions were achieved even under limited camera pose conditions.

The work highlights the importance of:

* Robust environment setup
* Careful data preprocessing
* Visual validation during training

This repository can serve as a **reference implementation** for students and researchers exploring NeRF-based 3D reconstruction.

---

## 👤 Author

**Pediredla Suman**
3D Reconstruction with Nerfstudio Project
