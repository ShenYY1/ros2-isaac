# DiffusionDrive-ROS2-SimDeploy 项目任务书 v1.0

## 0. 文档说明

本文档是本项目的完整任务书，用于指导两个月内完成一个可上传 GitHub、可展示、可写入简历、可在面试中讲清楚的自动驾驶工程项目。

本任务书不是简单待办列表，而是围绕每个模块回答以下问题：

- 为什么要做这个模块。
- 这个模块解决什么问题。
- 这个模块和其他模块的关系是什么。
- 需要学习哪些理论。
- 需要掌握哪些工具。
- 输入是什么。
- 输出是什么。
- 接口怎么设计。
- 具体实现流程是什么。
- 预期结果是什么。
- 如何验证结果。
- 可能遇到哪些风险。
- 如果失败，应该如何降级处理。

项目执行原则：

- 所有代码、理论笔记、实验记录由项目作者亲自学习、亲自撰写、亲自提交。
- AI 只负责引导、解释、拆解任务、审阅方案、分析错误和提供学习路线。
- 项目不追求盲目堆模块，而是追求主线完整、结果真实、过程可解释。
- 项目不以超过论文原始指标为目标，而以完整复现、工程闭环、部署分析和可讲解性为目标。

---

## 1. 项目名称

中文名称：

**基于 DiffusionDrive 的端到端自动驾驶复现、ROS2/Isaac Sim 仿真闭环与 TensorRT 量化部署项目**

英文名称：

**DiffusionDrive-ROS2-SimDeploy**

推荐 GitHub 仓库名：

```text
diffusiondrive-ros2-simdeployment
```

---

## 2. 项目背景

传统自动驾驶系统通常被拆分为多个模块：

```text
感知 perception
    ↓
预测 prediction
    ↓
规划 planning
    ↓
控制 control
```

这种模块化方案的优点是结构清楚、每个模块容易单独调试，但它也存在问题：

- 每个模块的误差会向后传递，形成误差累积。
- 模块之间接口复杂，系统集成成本高。
- 每个模块单独优化，不一定能保证最终驾驶行为最优。
- 在复杂长尾场景下，人工规则和手工设计模块很难覆盖所有情况。

端到端自动驾驶尝试使用统一模型直接学习从场景输入到未来轨迹或驾驶动作的映射关系。它的核心思想是：

```text
输入传感器 / 场景信息
    ↓
神经网络模型
    ↓
未来轨迹 / 控制动作
```

DiffusionDrive 是 CVPR 2025 Highlight 工作，提出使用 truncated diffusion model 做端到端自动驾驶轨迹规划。它把自动驾驶规划看成未来轨迹生成问题，并通过减少扩散采样步数提高实时性。

本项目选择 DiffusionDrive 作为主参考对象，不从零发明模型，而是复现、理解并工程化增强该方法。

---

## 3. 项目核心定位

本项目定位为：

> 以 DiffusionDrive 为核心论文，完成端到端自动驾驶轨迹规划方法的复现、训练实验、ROS2 工程封装、Isaac Sim 仿真闭环和 TensorRT 量化部署。

项目不是：

- 不是完整商用自动驾驶系统。
- 不是真实车辆上车项目。
- 不是从零训练大模型项目。
- 不是机械臂或抓取项目。
- 不是 VLM/VLA 大模型项目。
- 不是 YOLO 感知检测项目。

项目是：

- 端到端自动驾驶论文复现项目。
- 自动驾驶数据集学习项目。
- ROS2 工程接口项目。
- Isaac Sim 仿真闭环项目。
- 模型部署与量化项目。
- 强化学习基础扩展项目。

---

## 4. 项目总目标

两个月内完成一个完整工程闭环：

```text
DiffusionDrive 论文学习
        ↓
官方代码复现
        ↓
nuScenes 数据理解、推理、小规模训练
        ↓
ROS2 推理节点封装
        ↓
Isaac Sim 仿真闭环
        ↓
ONNX / TensorRT FP16 / TensorRT INT8 量化部署
        ↓
实验指标分析
        ↓
PPO 强化学习扩展 baseline
        ↓
GitHub 项目整理与简历化
```

最终应该能够回答：

- 我复现了哪篇论文。
- 这篇论文解决什么问题。
- 模型输入输出是什么。
- nuScenes 数据如何组织。
- 模型如何训练和推理。
- 如何把离线模型封装进 ROS2。
- 如何让模型控制 Isaac Sim 中的智能体。
- 如何把 PyTorch 模型导出到 ONNX。
- 如何使用 TensorRT 做 FP16 和 INT8 加速。
- 量化前后速度、显存、误差有什么变化。
- PPO 强化学习 baseline 和端到端轨迹规划有什么区别。

---

## 5. 项目边界

### 5.1 必须完成

- GitHub 仓库。
- README。
- 完整任务书。
- 环境配置文档。
- DiffusionDrive 论文笔记。
- DiffusionDrive 官方代码推理复现。
- nuScenes 数据集学习笔记。
- 小规模训练或 fine-tune 实验。
- ROS2 自定义轨迹消息。
- ROS2 模型推理节点。
- Isaac Sim 与 ROS2 通信。
- Isaac Sim 闭环控制演示。
- PyTorch 到 ONNX 导出。
- ONNX Runtime 推理。
- TensorRT FP16 部署。
- TensorRT INT8 量化尝试。
- 性能评估报告。
- PPO 强化学习扩展 baseline。
- 演示视频。
- 简历描述。
- 面试讲解稿。

### 5.2 明确不做

- 不做 YOLO 目标检测。
- 不做 VLM/VLA 主线。
- 不训练大语言模型。
- 不训练视觉语言动作模型。
- 不做机械臂、夹爪、MoveIt。
- 不做真实车辆上车部署。
- 不复现多个大型论文。
- 不从零训练完整 DiffusionDrive。
- 不下载和训练完整 nuScenes 全量数据作为第一目标。
- 不追求超过原论文指标。

### 5.3 可选增强

如果时间允许，可以增加：

- 更多 nuScenes 可视化。
- Isaac Sim 多场景变体。
- 租服务器做更长时间 fine-tune。
- TensorRT INT8 更完整校准。
- PPO baseline 可视化动画。
- BridgeAD、BridgeDrive、VAD 的阅读对比。

---

## 6. 项目最终交付物

最终仓库应包含：

```text
diffusiondrive-ros2-simdeployment/
├── README.md
├── docs/
│   ├── project_task_book.md
│   ├── environment_setup.md
│   ├── learning_roadmap.md
│   ├── paper/
│   │   └── diffusiondrive_notes.md
│   ├── dataset/
│   │   └── nuscenes_notes.md
│   ├── model/
│   │   └── diffusiondrive_architecture.md
│   ├── ros2/
│   │   └── ros2_interface_design.md
│   ├── isaac_sim/
│   │   └── isaac_closed_loop_design.md
│   ├── deployment/
│   │   └── tensorrt_quantization_notes.md
│   └── experiments/
│       ├── inference_log.md
│       ├── training_log.md
│       ├── isaac_closed_loop_log.md
│       └── quantization_report.md
├── third_party/
│   └── DiffusionDrive/
├── ros2_ws/
│   └── src/
│       ├── e2e_interfaces/
│       ├── e2e_inference_node/
│       ├── trajectory_controller/
│       └── e2e_bringup/
├── isaac_sim/
│   ├── scenes/
│   ├── scripts/
│   └── configs/
├── deployment/
│   ├── onnx/
│   ├── tensorrt/
│   └── benchmark/
├── experiments/
│   ├── nuscenes_inference/
│   ├── training_finetune/
│   ├── isaac_closed_loop/
│   └── quantization/
├── extensions/
│   └── rl_ppo_baseline/
└── results/
    ├── figures/
    ├── videos/
    └── metrics/
```

---

## 7. 总体技术架构

### 7.1 论文复现链路

```text
nuScenes 数据集
    ↓
DiffusionDrive 数据处理
    ↓
模型加载 checkpoint
    ↓
端到端轨迹规划推理
    ↓
轨迹预测结果
    ↓
L2 error / collision rate / 可视化
```

### 7.2 ROS2 + Isaac Sim 闭环链路

```text
Isaac Sim 仿真环境
    ↓ 发布
/sensor/camera/front, /ego/status, /odom
    ↓
ROS2 推理节点
    ↓
/planning/trajectory
    ↓
trajectory_controller
    ↓
/control/cmd
    ↓
Isaac Sim 车辆执行
    ↓
环境状态更新
```

### 7.3 模型部署链路

```text
PyTorch FP32
    ↓
ONNX FP32
    ↓
ONNX Runtime
    ↓
TensorRT FP16
    ↓
TensorRT INT8
    ↓
性能对比报告
```

### 7.4 强化学习扩展链路

```text
Gymnasium 2D 环境
    ↓
定义状态、动作、奖励
    ↓
Stable-Baselines3 PPO
    ↓
训练策略
    ↓
reward 曲线
    ↓
成功率 / 碰撞率
```

---

## 8. 模块 0：项目工程管理模块

### 8.1 模块目标

建立一个结构清晰、可长期维护、适合作为简历项目展示的 GitHub 仓库。

### 8.2 为什么要做

如果项目只有代码，没有文档、实验记录和结果展示，面试时很难证明作者真正理解项目。工程管理模块用于保证：

- 项目结构清楚。
- 每个阶段有记录。
- 每次实验可追踪。
- 每个模块有说明。
- 简历描述有真实依据。

### 8.3 需要学习

- Git。
- GitHub。
- commit。
- branch。
- remote。
- push。
- pull。
- clone。
- README。
- Markdown。
- `.gitignore`。
- issue。
- release。

### 8.4 推荐提交规范

```text
docs: add project task book
docs: add environment setup
docs: add diffusiondrive paper notes
feat: add ros2 trajectory message
feat: add inference node
feat: add trajectory controller
exp: add nuscenes inference results
exp: add training log
deploy: export model to onnx
deploy: add tensorrt fp16 benchmark
deploy: add int8 calibration script
fix: correct ros2 bridge config
```

这些前缀含义：

- `docs`：文档更新。
- `feat`：新增功能。
- `fix`：修复问题。
- `exp`：实验记录。
- `deploy`：部署相关。
- `test`：测试相关。
- `refactor`：代码重构。

### 8.5 具体任务

1. 创建 GitHub 仓库。
2. 初始化目录结构。
3. 编写 README 初版。
4. 编写任务书。
5. 编写环境配置文档。
6. 添加 `.gitignore`。
7. 建立 `docs/` 文档目录。
8. 建立 `results/` 结果目录。
9. 每完成一个阶段提交一次。
10. 每次实验写实验记录。

### 8.6 预期结果

项目仓库打开后，别人可以通过 README 快速知道：

- 这是一个什么项目。
- 参考了什么论文。
- 做了哪些模块。
- 怎么安装环境。
- 怎么运行推理。
- 怎么运行 ROS2 闭环。
- 怎么查看量化结果。
- 项目的结果和局限是什么。

---

## 9. 模块 1：环境部署与计算机基础模块

### 9.1 模块目标

搭建项目需要的全部基础环境，并理解每个环境组件的作用。

### 9.2 为什么要做

本项目涉及多个复杂工具：

- Ubuntu。
- NVIDIA 驱动。
- CUDA。
- PyTorch。
- DiffusionDrive。
- ROS2。
- Isaac Sim。
- ONNX。
- TensorRT。

如果环境混乱，后续复现、训练、仿真、部署都会失败。

### 9.3 环境分层

```text
操作系统层：Ubuntu
显卡驱动层：NVIDIA Driver
GPU 计算层：CUDA / cuDNN
Python 环境层：conda / pip
深度学习层：PyTorch
论文复现层：DiffusionDrive
机器人通信层：ROS2
仿真层：Isaac Sim
部署层：ONNX / TensorRT
工程管理层：Git / GitHub
```

### 9.4 需要学习的计算机基础

#### 9.4.1 操作系统基础

需要学习：

- Windows 和 Linux 的区别。
- Ubuntu 是什么。
- 双系统是什么。
- 文件系统是什么。
- 绝对路径和相对路径。
- 用户权限。
- `sudo` 的作用。
- 进程和线程。
- 环境变量。
- 终端和 shell。

#### 9.4.2 Linux 命令

需要学习：

```bash
pwd
```

作用：显示当前所在目录。

```bash
ls
```

作用：列出当前目录下的文件和文件夹。

```bash
cd
```

作用：切换目录。

```bash
mkdir
```

作用：创建目录。

```bash
rm
```

作用：删除文件或目录。该命令有风险，使用前必须确认路径。

```bash
cp
```

作用：复制文件或目录。

```bash
mv
```

作用：移动文件，也可以用于重命名。

```bash
chmod
```

作用：修改文件权限。

```bash
apt
```

作用：Ubuntu 的软件包管理工具，用来安装、更新、删除系统软件包。

#### 9.4.3 Python 环境基础

需要学习：

- Python 解释器是什么。
- pip 是什么。
- conda 是什么。
- 虚拟环境为什么重要。
- requirements.txt 是什么。
- package version 是什么。
- 为什么不同项目要使用不同环境。

#### 9.4.4 GPU 基础

需要学习：

- GPU 为什么适合深度学习。
- 显存是什么。
- CUDA 是什么。
- CUDA Driver 和 CUDA Toolkit 的区别。
- cuDNN 是什么。
- TensorRT 是什么。
- FP32、FP16、INT8 是什么。

### 9.5 关键命令说明

```bash
nvidia-smi
```

作用：

查看 NVIDIA 驱动是否正常、显卡型号、显存占用、当前 GPU 进程、驱动版本和驱动支持的 CUDA 版本。

注意：

`nvidia-smi` 显示的 CUDA 版本是驱动支持的最高 CUDA 运行版本，不一定等于本机安装的 CUDA Toolkit 版本。

```bash
nvcc --version
```

作用：

查看 CUDA Toolkit 编译器版本。这个命令显示的是安装在系统中的 CUDA 编译工具版本。

```bash
python --version
```

作用：

查看当前终端正在使用的 Python 解释器版本。

```bash
conda env list
```

作用：

查看当前机器上已有的 conda 虚拟环境，防止把依赖安装到错误环境。

```bash
pip list
```

作用：

查看当前 Python 环境中安装了哪些 Python 包。

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

作用：

验证 PyTorch 是否能访问 CUDA GPU。如果输出 `True`，说明 PyTorch 可以使用 GPU。

```bash
ros2 topic list
```

作用：

查看当前 ROS2 系统中存在的 topic。

```bash
ros2 topic echo /topic_name
```

作用：

打印某个 ROS2 topic 的实时消息，用于检查数据是否发布正常。

### 9.6 具体任务

1. 安装 Ubuntu 双系统。
2. 给 Ubuntu 分配足够磁盘空间。
3. 安装 NVIDIA 驱动。
4. 验证 `nvidia-smi`。
5. 安装 CUDA。
6. 安装 conda。
7. 创建项目虚拟环境。
8. 安装 PyTorch。
9. 验证 PyTorch GPU。
10. 安装 ROS2。
11. 跑通 ROS2 talker/listener demo。
12. 安装 Isaac Sim。
13. 安装 TensorRT。
14. 记录所有版本。

### 9.7 预期结果

`docs/environment_setup.md` 中需要记录：

- 系统版本。
- 显卡型号。
- 驱动版本。
- CUDA 版本。
- PyTorch 版本。
- ROS2 版本。
- Isaac Sim 版本。
- TensorRT 版本。
- 安装命令。
- 每条命令的作用。
- 验证方式。
- 遇到的问题。
- 解决方式。

---

## 10. 模块 2：DiffusionDrive 论文学习模块

### 10.1 模块目标

读懂 DiffusionDrive 论文的核心思想、模型结构、训练方式、推理方式和评估指标。

### 10.2 为什么要做

如果只会运行代码，不理解论文，项目在面试中很容易被质疑为“只是跑别人的仓库”。论文学习模块用于证明作者理解端到端自动驾驶和扩散式轨迹规划的理论基础。

### 10.3 主参考论文

```text
DiffusionDrive: Truncated Diffusion Model for End-to-End Autonomous Driving
CVPR 2025 Highlight
```

### 10.4 待学习理论

#### 10.4.1 端到端自动驾驶

需要理解：

- 什么是端到端自动驾驶。
- 端到端和模块化自动驾驶的区别。
- 为什么端到端可以减少误差累积。
- 为什么端到端难解释。
- 为什么端到端需要大量数据。
- 端到端模型常见输出为什么是轨迹。

#### 10.4.2 轨迹规划

需要理解：

- ego vehicle 是什么。
- ego trajectory 是什么。
- future trajectory 是什么。
- 轨迹点通常包含什么。
- 为什么自动驾驶常预测未来 1s、2s、3s 轨迹。
- 轨迹如何转成控制命令。

#### 10.4.3 扩散模型

需要理解：

- diffusion model 是什么。
- forward diffusion 是什么。
- reverse denoising 是什么。
- 为什么扩散模型可以生成多样化结果。
- diffusion policy 是什么。
- 为什么扩散模型能用于轨迹生成。
- 传统扩散模型为什么推理慢。
- truncated diffusion 如何减少采样步数。

#### 10.4.4 评价指标

需要理解：

- L2 trajectory error。
- collision rate。
- inference latency。
- FPS。
- open-loop evaluation。
- closed-loop evaluation。
- pseudo closed-loop evaluation。

### 10.5 具体任务

1. 阅读论文摘要。
2. 阅读 introduction，理解问题背景。
3. 阅读 method，理解模型结构。
4. 阅读 experiments，理解评估方式。
5. 总结论文创新点。
6. 总结论文局限性。
7. 画出模型流程图。
8. 写出模型输入和输出。
9. 写出训练目标。
10. 写出推理流程。
11. 写论文笔记。

### 10.6 预期结果

完成文档：

```text
docs/paper/diffusiondrive_notes.md
```

该文档应回答：

- DiffusionDrive 想解决什么问题。
- 为什么使用 diffusion。
- truncated diffusion 的意义是什么。
- 它和传统端到端规划方法有什么区别。
- 它在 nuScenes 上如何评估。
- 它适合工程部署的原因是什么。

---

## 11. 模块 3：nuScenes 数据集模块

### 11.1 模块目标

理解 nuScenes 数据结构，并能够使用 nuScenes 运行 DiffusionDrive 推理和小规模训练实验。

### 11.2 为什么要做

DiffusionDrive 的复现依赖 nuScenes。nuScenes 是自动驾驶领域常用真实数据集，理解它有助于理解自动驾驶数据、轨迹、传感器和坐标系。

### 11.3 nuScenes 提供什么

nuScenes 包含：

- 多相机图像。
- 激光雷达点云。
- 毫米波雷达。
- 车辆位姿。
- 传感器标定。
- 目标 3D 标注。
- 场景序列。
- 未来轨迹。
- CAN bus 数据。

### 11.4 待学习概念

- `scene`：一段完整驾驶场景。
- `sample`：某一个时间点的数据。
- `sample_data`：某个传感器在某个时刻的数据。
- `ego_pose`：自车在全局坐标系中的位姿。
- `calibrated_sensor`：传感器外参和内参。
- `annotation`：目标标注。
- `CAN bus`：车辆状态数据，例如速度、加速度。
- `future trajectory`：未来自车轨迹。
- 坐标系转换：sensor、ego、global 坐标系之间的转换。

### 11.5 数据流程

```text
下载 nuScenes mini
    ↓
安装 nuScenes devkit
    ↓
读取 scene
    ↓
读取 sample
    ↓
读取 camera / lidar / ego_pose
    ↓
可视化图像和点云
    ↓
提取历史轨迹和未来轨迹
    ↓
用于 DiffusionDrive 推理或训练
```

### 11.6 具体任务

1. 下载 nuScenes mini。
2. 安装 nuScenes devkit。
3. 读取一个 scene。
4. 打印 scene 信息。
5. 读取一个 sample。
6. 可视化相机图像。
7. 可视化 lidar 点云。
8. 可视化 ego 历史轨迹。
9. 可视化 ego 未来轨迹。
10. 学习坐标转换。
11. 写 nuScenes 学习笔记。

### 11.7 预期结果

完成文档：

```text
docs/dataset/nuscenes_notes.md
```

该文档应回答：

- nuScenes 一帧数据包含什么。
- scene、sample、sample_data 的关系是什么。
- ego_pose 有什么作用。
- calibrated_sensor 有什么作用。
- 为什么模型需要历史信息和未来轨迹。
- DiffusionDrive 如何使用 nuScenes 数据。

---

## 12. 模块 4：DiffusionDrive 官方代码复现模块

### 12.1 模块目标

跑通 DiffusionDrive 官方代码、官方权重和推理流程。

### 12.2 为什么要做

这是项目主体的第一步。只有官方代码跑通，后续训练、部署、ROS2 封装、Isaac Sim 闭环才有基础。

### 12.3 输入

- DiffusionDrive 官方仓库。
- 官方 checkpoint。
- 官方 config。
- nuScenes 数据。

### 12.4 输出

- 预测轨迹。
- 推理日志。
- 评估指标。
- 可视化结果。

### 12.5 需要学习

- 仓库目录结构。
- config 文件。
- checkpoint。
- dataset pipeline。
- model forward。
- inference script。
- evaluation script。
- log 文件。

### 12.6 具体任务

1. 克隆官方仓库。
2. 切换 nuScenes 相关分支。
3. 创建 conda 环境。
4. 安装依赖。
5. 下载 checkpoint。
6. 配置 nuScenes 路径。
7. 跑通官方推理脚本。
8. 保存推理日志。
9. 保存预测轨迹。
10. 保存可视化图片。
11. 阅读关键代码。
12. 写复现记录。

### 12.7 预期结果

完成文档：

```text
docs/experiments/inference_log.md
docs/model/diffusiondrive_architecture.md
```

项目中应保存：

- 推理输出。
- 日志文件。
- 可视化图片。
- 关键代码阅读笔记。

---

## 13. 模块 5：小规模训练 / fine-tune 模块

### 13.1 模块目标

完成小规模训练或 fine-tune，用来体现深度学习训练能力。

### 13.2 为什么要做

只跑官方推理容易被认为只是运行别人的 demo。训练实验可以体现作者理解：

- 数据加载。
- 训练配置。
- loss。
- optimizer。
- checkpoint。
- 验证集评估。
- 显存限制。

### 13.3 不追求什么

- 不从零训练完整模型。
- 不追求达到原论文指标。
- 不做全量 nuScenes 大规模训练。
- 不把训练作为项目最大风险点。

### 13.4 待学习内容

- batch。
- epoch。
- learning rate。
- optimizer。
- scheduler。
- loss function。
- checkpoint。
- validation。
- overfitting。
- fine-tune。
- mixed precision。
- GPU memory。

### 13.5 训练流程

```text
准备小规模数据
    ↓
修改训练配置
    ↓
加载预训练权重
    ↓
开始 fine-tune
    ↓
记录 training loss
    ↓
保存 checkpoint
    ↓
验证集评估
    ↓
分析结果
```

### 13.6 具体任务

1. 阅读训练脚本。
2. 理解训练配置。
3. 准备小规模数据。
4. 尝试加载预训练权重。
5. 跑通一个短周期训练。
6. 保存 loss 曲线。
7. 保存 checkpoint。
8. 在验证集上测试。
9. 分析训练是否过拟合。
10. 写训练日志。

### 13.7 预期结果

完成文档：

```text
docs/experiments/training_log.md
```

该文档应包含：

- 使用的数据规模。
- 训练配置。
- batch size。
- learning rate。
- 训练时长。
- loss 曲线。
- checkpoint 路径。
- 验证指标。
- 训练问题和分析。

---

## 14. 模块 6：ROS2 接口与推理节点模块

### 14.1 模块目标

将 DiffusionDrive 模型封装成 ROS2 节点，使其能够参与仿真系统通信。

### 14.2 为什么要做

论文代码通常是离线脚本。ROS2 节点可以把模型变成系统中的一个模块，让它能够接收输入、发布输出，并与 Isaac Sim 交互。

### 14.3 因果关系

```text
模型要控制仿真车辆
    ↓
需要实时接收状态
    ↓
需要输出轨迹
    ↓
需要统一消息格式
    ↓
ROS2 提供 topic / msg / launch
    ↓
模型封装成推理节点
```

### 14.4 待学习内容

- ROS2 workspace。
- package。
- node。
- topic。
- msg。
- service。
- action。
- launch。
- parameter。
- QoS。
- `sensor_msgs/Image`。
- `nav_msgs/Odometry`。
- 自定义 msg。

### 14.5 输入接口

初步设计：

```text
/sensor/camera/front
/ego/status
/route/command
```

含义：

- `/sensor/camera/front`：前视相机图像。
- `/ego/status`：自车状态，例如速度、位姿。
- `/route/command`：高层路线命令，例如直行、左转、右转。

### 14.6 输出接口

```text
/planning/trajectory
/control/cmd
```

含义：

- `/planning/trajectory`：模型预测的未来轨迹。
- `/control/cmd`：轨迹控制器转换后的控制命令。

### 14.7 自定义消息设计

轨迹点消息：

```text
float32 x
float32 y
float32 yaw
float32 velocity
float32 time_from_start
```

轨迹消息：

```text
std_msgs/Header header
TrajectoryPoint[] points
float32 confidence
```

### 14.8 推理节点流程

```text
接收 ROS2 输入
    ↓
数据缓存和同步
    ↓
图像 / 状态预处理
    ↓
模型推理
    ↓
轨迹后处理
    ↓
发布 /planning/trajectory
```

### 14.9 具体任务

1. 创建 `ros2_ws`。
2. 创建 `e2e_interfaces`。
3. 定义轨迹消息。
4. 创建 `e2e_inference_node`。
5. 加载 PyTorch 模型。
6. 接收输入 topic。
7. 做数据预处理。
8. 执行模型推理。
9. 发布轨迹 topic。
10. 编写 launch 文件。
11. 编写接口协议文档。

### 14.10 预期结果

完成：

```text
docs/ros2/ros2_interface_design.md
```

并且能够：

- 启动 ROS2 推理节点。
- 通过 `ros2 topic list` 看到 topic。
- 通过 `ros2 topic echo /planning/trajectory` 看到轨迹输出。
- 解释每个 topic 谁发布、谁订阅、消息格式是什么。

---

## 15. 模块 7：Isaac Sim 仿真闭环模块

### 15.1 模块目标

在 Isaac Sim 中建立车辆或移动智能体仿真环境，并通过 ROS2 与模型推理节点形成闭环。

### 15.2 为什么要做

nuScenes 是离线数据，不能让模型动作影响下一帧环境。Isaac Sim 可以提供可交互仿真环境，让模型输出真正驱动车辆运动。

### 15.3 什么是闭环

闭环不是单次推理，而是：

```text
环境产生观测
    ↓
模型输出动作或轨迹
    ↓
控制器执行动作
    ↓
环境状态改变
    ↓
模型再次观察新状态
```

### 15.4 待学习内容

- Isaac Sim 基础界面。
- USD 场景。
- 车辆模型。
- 移动智能体。
- 相机传感器。
- ROS2 Bridge。
- `/cmd_vel`。
- odometry。
- 仿真时间。
- 坐标系。
- 轨迹跟踪控制。

### 15.5 Isaac Sim 输出

```text
/sensor/camera/front
/ego/status
/odom
/collision/status
```

### 15.6 Isaac Sim 输入

```text
/control/cmd
```

### 15.7 闭环流程

```text
Isaac Sim 发布车辆状态
    ↓
ROS2 推理节点接收状态
    ↓
DiffusionDrive 输出未来轨迹
    ↓
trajectory_controller 生成控制命令
    ↓
Isaac Sim 执行控制
    ↓
车辆移动并产生新状态
```

### 15.8 具体任务

1. 安装 Isaac Sim。
2. 配置 ROS2 Bridge。
3. 创建简单道路或园区场景。
4. 添加车辆或移动智能体。
5. 添加前视相机。
6. 发布车辆状态。
7. 接收控制命令。
8. 编写 trajectory controller。
9. 接入模型推理节点。
10. 录制闭环演示视频。
11. 写闭环测试记录。

### 15.9 预期结果

完成：

```text
docs/isaac_sim/isaac_closed_loop_design.md
docs/experiments/isaac_closed_loop_log.md
```

并且能够展示：

- Isaac Sim 和 ROS2 正常通信。
- ROS2 能控制 Isaac Sim 车辆。
- 模型输出轨迹可以影响车辆运动。
- 有完整闭环演示视频。

---

## 16. 模块 8：模型量化部署模块

### 16.1 模块目标

完成从 PyTorch 到 ONNX、ONNX Runtime、TensorRT FP16、TensorRT INT8 的部署链路，并分析量化对速度和效果的影响。

### 16.2 为什么要做

自动驾驶系统需要实时性。论文模型如果只能离线慢速推理，工程价值不足。模型量化和部署可以体现深度学习工程能力。

### 16.3 部署路线

```text
PyTorch FP32
    ↓
ONNX FP32
    ↓
ONNX Runtime
    ↓
TensorRT FP16
    ↓
TensorRT INT8
```

### 16.4 待学习内容

- ONNX。
- ONNX Runtime。
- TensorRT。
- FP32。
- FP16。
- INT8。
- calibration。
- engine。
- dynamic shape。
- static shape。
- latency。
- throughput。
- FPS。
- GPU memory。

### 16.5 核心概念解释

FP32：

32 位浮点数，精度较高，训练和推理都常用，但速度和显存占用相对较高。

FP16：

16 位浮点数，精度比 FP32 低，但速度更快，显存占用更低，适合 NVIDIA GPU 上的推理加速。

INT8：

8 位整数，模型更小、速度更快，但精度损失更明显，需要 calibration 数据减少误差。

ONNX：

开放神经网络交换格式，用于把 PyTorch 模型导出到其他推理框架。

TensorRT：

NVIDIA 的高性能推理优化框架，可以做算子融合、精度降低、内存优化和推理加速。

Calibration：

INT8 量化时，用一批代表性输入数据统计激活范围，帮助模型从浮点数映射到整数时减少误差。

### 16.6 具体任务

1. 分析模型输入输出 shape。
2. 导出 ONNX。
3. 检查 ONNX 模型。
4. 对比 PyTorch 输出和 ONNX 输出。
5. 使用 ONNX Runtime 推理。
6. 构建 TensorRT FP16 engine。
7. 准备 INT8 calibration 数据。
8. 构建 TensorRT INT8 engine。
9. 记录模型大小。
10. 记录平均推理延迟。
11. 记录 FPS。
12. 记录显存占用。
13. 对比轨迹误差。
14. 写量化部署报告。

### 16.7 指标表设计

```text
模型格式 | 模型大小 | 平均延迟 | FPS | 显存占用 | L2 error | 备注
PyTorch FP32 | | | | | |
ONNX Runtime | | | | | |
TensorRT FP16 | | | | | |
TensorRT INT8 | | | | | |
```

### 16.8 预期结果

必须完成：

- PyTorch FP32 推理。
- ONNX 导出。
- ONNX Runtime 推理。
- TensorRT FP16 推理。

尽量完成：

- TensorRT INT8 推理。

如果 INT8 失败，也要记录：

- 哪一步失败。
- 报错是什么。
- 是否是算子不支持。
- 是否是 dynamic shape 问题。
- 后续如何解决。

---

## 17. 模块 9：实验评估模块

### 17.1 模块目标

通过指标、图像、曲线、表格和视频证明项目成果。

### 17.2 为什么要做

项目不能只说“我跑通了”。必须用结果说明：

- 模型是否正常推理。
- 训练是否有效。
- ROS2 是否稳定。
- Isaac Sim 是否闭环。
- 量化是否加速。
- 量化是否影响误差。

### 17.3 评估内容

#### 17.3.1 DiffusionDrive 复现指标

- L2 trajectory error。
- collision rate。
- 推理时间。
- 轨迹可视化结果。

#### 17.3.2 训练实验指标

- training loss。
- validation loss。
- checkpoint。
- 是否过拟合。
- GPU 显存占用。

#### 17.3.3 ROS2 指标

- topic 是否正常。
- 节点是否稳定。
- 消息频率。
- 推理节点延迟。
- launch 是否可一键启动。

#### 17.3.4 Isaac Sim 闭环指标

- 车辆是否能运动。
- 控制是否稳定。
- 是否抖动。
- 是否能完成简单路线。
- 仿真 FPS。
- 闭环演示视频。

#### 17.3.5 量化部署指标

- 模型大小。
- latency。
- FPS。
- 显存。
- 轨迹误差变化。
- 闭环控制效果变化。

### 17.4 预期文档

```text
docs/experiments/inference_log.md
docs/experiments/training_log.md
docs/experiments/isaac_closed_loop_log.md
docs/experiments/quantization_report.md
```

---

## 18. 模块 10：强化学习 PPO 扩展模块

### 18.1 模块目标

实现一个小型 PPO baseline，用于体现强化学习基础能力。

### 18.2 模块定位

强化学习不是项目主线，只是扩展模块。

主线是：

```text
DiffusionDrive + ROS2 + Isaac Sim + TensorRT
```

扩展是：

```text
Gymnasium + PPO baseline
```

### 18.3 为什么要做

主线体现深度学习、端到端规划和工程部署。PPO baseline 用于证明作者理解强化学习中的：

- state。
- action。
- reward。
- policy。
- value function。
- rollout。
- episode。
- training curve。

### 18.4 环境设计

状态：

```text
goal_distance
goal_angle
linear_velocity
angular_velocity
nearest_obstacle_distance
```

动作：

```text
linear_velocity_command
angular_velocity_command
```

奖励：

```text
靠近目标：加分
远离目标：扣分
碰撞：大扣分
到达目标：大奖励
动作过大：小扣分
```

### 18.5 待学习内容

- Gymnasium。
- Stable-Baselines3。
- PPO。
- policy。
- value function。
- reward function。
- episode。
- rollout。
- exploration。
- reward curve。

### 18.6 不做内容

- 不做 Isaac Sim 大规模强化学习。
- 不做图像输入 RL。
- 不接入 DiffusionDrive 主线。
- 不做复杂多智能体 RL。

### 18.7 具体任务

1. 创建 2D 环境。
2. 定义 observation space。
3. 定义 action space。
4. 编写 reward function。
5. 使用 PPO 训练。
6. 保存 reward 曲线。
7. 测试成功率。
8. 测试碰撞率。
9. 写强化学习总结文档。

### 18.8 预期结果

完成：

```text
extensions/rl_ppo_baseline/
docs/experiments/rl_ppo_baseline_log.md
```

并且能够解释：

- PPO 如何训练。
- reward 为什么这样设计。
- 强化学习和端到端轨迹规划有什么区别。
- 为什么本项目不把 RL 作为主线。

---

## 19. 两个月执行计划

### 第 1 周：Ubuntu、GitHub、基础环境

目标：

- 安装 Ubuntu 双系统。
- 配置 NVIDIA 驱动。
- 配置 CUDA。
- 配置 conda。
- 安装 PyTorch。
- 创建 GitHub 仓库。
- 编写 README 初版。
- 编写任务书。

产出：

- `README.md`
- `docs/project_task_book.md`
- `docs/environment_setup.md`

### 第 2 周：DiffusionDrive 复现

目标：

- 克隆官方代码。
- 配置环境。
- 下载权重。
- 准备 nuScenes mini。
- 跑通官方推理。
- 写论文笔记。

产出：

- `docs/paper/diffusiondrive_notes.md`
- `docs/experiments/inference_log.md`
- 推理结果截图。

### 第 3 周：nuScenes 与训练实验

目标：

- 学习 nuScenes 数据结构。
- 可视化数据。
- 跑通小规模训练或 fine-tune。
- 保存 loss 曲线和 checkpoint。

产出：

- `docs/dataset/nuscenes_notes.md`
- `docs/experiments/training_log.md`
- loss 曲线。
- checkpoint。

### 第 4 周：ROS2 推理节点

目标：

- 创建 ROS2 工作空间。
- 定义自定义轨迹消息。
- 封装模型推理节点。
- 发布轨迹 topic。
- 编写接口协议文档。

产出：

- `ros2_ws/src/e2e_interfaces/`
- `ros2_ws/src/e2e_inference_node/`
- `docs/ros2/ros2_interface_design.md`

### 第 5 周：Isaac Sim 闭环

目标：

- 安装和配置 Isaac Sim。
- 配置 ROS2 Bridge。
- 创建仿真车辆场景。
- 实现 ROS2 控制车辆。
- 接入模型推理节点。
- 录制闭环视频。

产出：

- `docs/isaac_sim/isaac_closed_loop_design.md`
- `docs/experiments/isaac_closed_loop_log.md`
- 演示视频。

### 第 6 周：ONNX 与 TensorRT FP16

目标：

- 导出 ONNX。
- 验证 ONNX Runtime。
- 构建 TensorRT FP16 engine。
- 对比 PyTorch、ONNX、TensorRT FP16。

产出：

- ONNX 模型。
- TensorRT FP16 engine。
- benchmark 结果。

### 第 7 周：INT8 与 PPO 扩展

目标：

- 准备 INT8 calibration 数据。
- 尝试 TensorRT INT8。
- 完成量化报告。
- 实现 PPO baseline。

产出：

- INT8 engine 或失败分析。
- `docs/experiments/quantization_report.md`
- `extensions/rl_ppo_baseline/`

### 第 8 周：整理与简历化

目标：

- 完善 README。
- 整理架构图。
- 整理实验表格。
- 整理演示视频。
- 写项目总结。
- 写简历描述。
- 写面试讲解稿。

产出：

- 完整 GitHub 项目。
- 演示视频。
- 简历项目描述。
- 面试讲解稿。

---

## 20. 风险与应对

### 20.1 环境配置失败

风险：

DiffusionDrive、CUDA、PyTorch、TensorRT、Isaac Sim 版本不兼容。

应对：

- 优先使用官方推荐版本。
- 每一步都记录版本。
- 先跑通推理，再做训练。
- 必要时使用 Docker。

### 20.2 nuScenes 数据过大

风险：

数据占用空间大，下载时间长。

应对：

- 先使用 nuScenes mini。
- 不在第一阶段下载 full。
- 数据放到单独目录。
- 文档记录数据路径。

### 20.3 训练太慢

风险：

本地 RTX 4070 无法支撑完整训练。

应对：

- 不从零训练。
- 使用官方 checkpoint。
- 只做小规模 fine-tune。
- 必要时租服务器。

### 20.4 Isaac Sim 闭环困难

风险：

ROS2 Bridge、控制接口、仿真车辆控制可能配置复杂。

应对：

- 先用手写控制命令控制车辆。
- 再接轨迹控制器。
- 最后接模型推理节点。
- 分阶段验证。

### 20.5 TensorRT INT8 失败

风险：

模型算子不支持、shape 动态变化、校准数据不足。

应对：

- FP16 必须完成。
- INT8 可以作为尝试。
- 如果失败，记录失败原因和解决思路。

### 20.6 项目范围膨胀

风险：

加入太多模块导致主线失败。

应对：

- 主线固定为 DiffusionDrive、ROS2、Isaac Sim、TensorRT。
- PPO 只是扩展。
- 不加入 YOLO、VLM、VLA、机械臂。

---

## 21. README 内容要求

最终 README 至少包含：

1. 项目简介。
2. 项目架构图。
3. 技术栈。
4. 参考论文。
5. 环境配置。
6. 数据准备。
7. DiffusionDrive 复现结果。
8. 训练实验结果。
9. ROS2 推理节点说明。
10. Isaac Sim 闭环演示。
11. ONNX / TensorRT 部署结果。
12. PPO 扩展实验。
13. 项目局限性。
14. 后续计划。

---

## 22. 简历描述草稿

复现 CVPR 2025 Highlight 端到端自动驾驶方法 DiffusionDrive，基于 nuScenes 完成轨迹规划推理与小规模训练实验；设计 ROS2 推理节点并接入 Isaac Sim，实现端到端轨迹规划模型的仿真闭环控制；完成 PyTorch、ONNX、TensorRT FP16、TensorRT INT8 部署链路，分析量化前后推理延迟、显存占用和规划效果差异；额外实现 PPO 强化学习避障 baseline，用于对比端到端轨迹规划与奖励驱动策略学习方法。

---

## 23. 面试讲解重点

面试中应重点讲清楚：

- 为什么选择 DiffusionDrive。
- DiffusionDrive 和传统规划方法有什么区别。
- diffusion 为什么适合轨迹生成。
- truncated diffusion 为什么能提升实时性。
- nuScenes 数据结构是什么。
- 小规模训练做了什么。
- ROS2 节点如何设计。
- Isaac Sim 闭环如何形成。
- TensorRT FP16 和 INT8 有什么区别。
- 量化后效果如何变化。
- PPO baseline 为什么作为扩展而不是主线。

---

## 24. 项目成功标准

本项目成功不以超过原论文指标为标准，而以以下标准判断：

- 是否跑通 DiffusionDrive。
- 是否理解论文。
- 是否使用 nuScenes。
- 是否完成训练实验。
- 是否完成 ROS2 推理节点。
- 是否完成 Isaac Sim 闭环。
- 是否完成 ONNX / TensorRT 部署。
- 是否有量化对比。
- 是否有 PPO 扩展。
- 是否有完整文档。
- 是否有演示视频。
- 是否能在面试中讲清楚每个模块的因果、流程、结果和问题。

---

## 25. 项目一句话总结

本项目围绕 DiffusionDrive 端到端自动驾驶轨迹规划方法，完成从论文复现、数据理解、训练实验到 ROS2/Isaac Sim 仿真闭环和 TensorRT 量化部署的完整工程实践，并通过 PPO baseline 补充强化学习能力展示。

