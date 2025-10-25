# LeRobot Sandwich-Making (IROS 2026)
[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)]()
[![ROS](https://img.shields.io/badge/ROS-ROS2%2FNoetic-informational)]()
[![License](https://img.shields.io/badge/License-Apache--2.0-green.svg)]()
[![Status](https://img.shields.io/badge/Status-Under%20Review-orange.svg)]()
A LeRobot-based food assembly pipeline for **sandwich-making** with a low-cost robot arm. This repository contains code, configs, and experiment scripts for our **IROS 2026** submission.
> **TL;DR:** We study multi-ingredient manipulation (bread, lettuce, tomato, cheese, etc.) using perception, skill primitives, and imitation/RL on LeRobot-compatible hardware. We release training/evaluation scripts and a minimal dataset specification for reproducibility.

## 1. Highlights
- **LeRobot-first:** Works out-of-the-box with LeRobot stack and low-cost arms.  
- **Food-safe manipulation:** Grasping and placing deformable and slippery slices.  
- **Learning-centric:** Supports imitation learning / behavior cloning; plug-in RL policies.  
- **Reproducibility:** Dataset spec and exact training configs are provided.

## 2. Repository Structure
```
lerobot-sandwich-iros26/
├─ lerobot_sandwich/
│  ├─ percep/           # perception modules (segmentation/detection/pose)
│  ├─ control/          # low-level control and motion primitives
│  ├─ policy/           # IL/RL policies, BC training and evaluation
│  ├─ sim/              # simulation wrappers (Isaac/Gym/MuJoCo if used)
│  ├─ ros/              # ROS nodes, launch files (ROS2 or Noetic)
│  └─ utils/            # common utils, logging, I/O
├─ configs/             # YAML configs for experiments
├─ scripts/             # data collection, training, evaluation scripts
├─ data/                # (gitignored) datasets / recordings (spec only)
├─ checkpoints/         # (gitignored) trained policies
├─ docs/                # figures, paper assets
└─ README.md

````
## 3. Setup
### 3.1 Dependencies
- Python 3.10+
- LeRobot (see official installation)
- ROS (ROS2 Humble / ROS Noetic)
- PyTorch / CUDA (optional for training)
- (Optional) RealSense or other RGB-D camera drivers
```bash
# Create environment
conda create -n lerobot-sandwich python=3.10 -y
conda activate lerobot-sandwich
# Install dependencies
pip install -r requirements.txt   # TODO: provide this file
# Install LeRobot
# Follow the official installation guide:
# https://github.com/huggingface/lerobot
````

### 3.2 ROS build

If using ROS2:

```bash
source /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash
```

## 4. Quick Start

### 4.1 Data Collection (Teleop or Scripted)

```bash
python scripts/collect_teleop.py \
  --save_dir data/sandwich_demo \
  --camera realsense \
  --ingredients bread,lettuce,tomato,cheese
```

### 4.2 Train (Behavior Cloning)

```bash
python scripts/train_bc.py \
  --data data/sandwich_demo \
  --cfg configs/bc_sandwich.yaml \
  --out checkpoints/bc_sandwich.pt
```

### 4.3 Evaluate on Robot

```bash
ros2 launch lerobot_sandwich/ros/eval.launch.py \
  policy:=checkpoints/bc_sandwich.pt \
  camera:=realsense \
  scene:=tabletop
```

## 5. Dataset Specification

* **Format:** RGB-D frames + robot states + gripper I/O + action sequences
* **Schema:** see `docs/dataset_schema.md` (TODO)
* **Notes:** All raw data are ignored by git (.gitignore). A minimal safe sample (no PII, food-only) is provided.

## 6. Results (WIP)

| Task         | Success @ 20 Trials | Notes       |
| ------------ | ------------------- | ----------- |
| Place bread  | 90 %                | Flat rigid  |
| Place cheese | 85 %                | Thin slice  |
| Place tomato | 70 %                | Wet surface |

> **Repro tips:** Fix random seeds, pin package versions, and record robot firmware/driver info.

## 7. Citation

If you find this repository useful, please cite:

```bibtex
@misc{zhao2026lerobot_sandwich,
  title  = {LeRobot-based Sandwich-making Manipulation},
  author = {Zhao, Hang (Allen)},
  year   = {2026},
  note   = {IROS 2026 submission code},
  url    = {https://github.com/AllenZhaoHang/lerobot-sandwich-iros26}
}
```

## 8. License

* **Code:** [Apache License 2.0](LICENSE)
* **Models / Datasets:** [CC BY 4.0](DATA_LICENSE) (unless stated otherwise)
  All components are released for academic and research use.

## 9. Acknowledgments

We thank the **LeRobot** community and all open-source contributors whose work made this project possible. Special thanks to collaborators and mentors who provided valuable feedback during development.

## 10. Contact

**Hang (Allen) Zhao**
Northeastern University – MSCS Program
Email: [zhao.hang1@northeastern.edu](mailto:zhao.hang1@northeastern.edu)
GitHub: [AllenZhaoHang](https://github.com/AllenZhaoHang)

```
::contentReference[oaicite:0]{index=0}
```
