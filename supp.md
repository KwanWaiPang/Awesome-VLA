## VLA方法总览

<!-- |  年份 |  单位  | 模型  |  方法  | 说明 | -->

|  年份 |  单位  | 模型  |  方法  | 说明 |
|:-----:|:-----:|:-----:|:-----:| ---------- |
|  2026 |  NTU  | [DynamicVLA](https://arxiv.org/pdf/2601.22153)  |  ViT+LLM+Action Expert/Flow Matching  | 动态物体抓取（滚动的橙子、移动的容器）：1. 紧凑架构（0.4B参数，FastViT/视觉编码器+ SmolLM2-360M/语音backbone+ 扩散式动作专家）；2.实时推理机制，连续推理（Continuous Inference）” 与 “潜变量感知动作流（Latent-aware Action Streaming）” 双机制； 3. 首个大规模动态物体操纵基准（Dynamic Object Manipulation, DOM， 200K 仿真 episode 与 2K 真实世界 episode）；在DOM上SR为38~60%对比PI0只有8~10% |
|  2026 |  蚂蚁灵波  | [LingBot-VLA](https://arxiv.org/pdf/2601.18692)  |  大小脑架构：VLM（大脑）+MoT作为动作专家  | 预训练VLM+Mixture-of-Transformers (MoT) /action expert，采用了 Flow Matching 方法来建模连续、平滑的动作轨迹；采用了一种基于视觉蒸馏的深度信息融合方法（不需要将深度图作为原始输入，将VLM提取的特征与专用深度模型 [LingBot-Depth](https://arxiv.org/pdf/2601.17895) 所生成的空间表征进行对齐）；训练数据涵盖 9 种主流双臂机器人配置、总计约 20,000 小时的真实世界操作数据;引入深度信息后，可实现玻璃花瓶（透明物体）插花；在多个机器人平台上，比PI0.5平均SR高4.28% |
|  2026 |  HIT  | [TwinBrainVLA](https://arxiv.org/pdf/2601.14133)  |  双脑/VLM+DiT（动作专家）  | “双脑”架构——冻结左脑负责语义，可训右脑负责控制;模型包含两个结构相同的VLM骨干（实验中使用了Qwen2.5-VL和Qwen3-VL系列）:左脑： 权重完全冻结。输入仅包含视觉图像和文本指令。它的任务是保持高质量的视觉语言表征; 右脑：权重全量微调。输入除了图像和文本外，还额外拼接了通过MLP编码的机器人本体状态。这是为了让右脑具备闭环控制所需的物理感知能力。|
|  2026 |  BeingBeyond  | [Being-H0.5](https://research.beingbeyond.com/projects/being-h05/being-h05.pdf)  |  Mixture-of-Transformers/专家混合模型（MoE）+action expert  | 超过 14,000 小时 的机器人操作数据与 16,000 小时 的人类视频数据，总训练 token 数突破 4000亿，规模达到 Qwen2.5‑VL 的十分之一;提出了统一动作空间架构，将双足人形、轮式底盘、桌面机械臂、夹爪、灵巧手等形态各异的机器人，映射到同一特征表示空间中，从而有效支撑跨本体联合训练与知识迁移。 |
|  2026 |  Berkeley  | [MomaGraph-R1](https://arxiv.org/pdf/2512.16909)  |  VLM  | MomaGraph-Scenes 数据集，6278张多视角家庭照片+1050 个任务场景图+ 350+ 家庭场景、93 种任务（开窗户、烧开水、开电视等）；通过给机器人构建任务导向图（sense graph），再按图规划任务执行导航与操作；模型为Qwen-2.5-VL-7B（作为planner，底层执行采用基本遥操技能库）+强化学习；星动纪元星动Q5测试了4个常见任务：开柜子、开微波炉、开电视、关灯|
|  2025 |  上海交通大学  | [Mantis](https://arxiv.org/pdf/2511.16175)  |  VLM  | 强调对VLA模型的语言监督(引入语言生成，保留VLM主干的理解与推理能力)，视觉预测与动作生成解耦(减轻主网络负担，提升fine-tune的收敛)，LIBERO benchmark上平均SR达96.7% |
|  2025 |  清华大学  | [Motus](https://arxiv.org/pdf/2512.13030)  |  VLM(Qwen3-VL-2B)+World Model(Wan2.2-5B)+action expert(Transformer)  | VLA和世界模型纳入了统一框架;研究基于 UniDiffuser（统一扩散生成框架）的建模理论，将“视频生成”和“动作生成”视为同一生成过程中的两个变量，并为二者设计了相互独立的噪声调度机制;采用了 MoT（Mixture-of-Transformers，专家混合 Transformer） 结构，将擅长理解语义的视觉语言模型和擅长建模动态的生成式视频模型作为不同“专家”引入同一框架;RoboTwin2.0 50个任务平均成功率为87～88%（比PI0.5高45%，比X-VLA高15%），真机验证也高于PI0.5 |
|  2025 |  北京人形机器人  | [XR-1](https://arxiv.org/pdf/2511.02776v1) |  VLM（双分支的向量化自编码器，PaliGemma+Gemma+action head）  | Unified Vision-Motion Codes (UVMC，多模态统一视觉-动作编码）将视觉观察、语言指令和机器人动作在统一的表征空间中进行学习；三阶段训练：多模态预训练→跨本体主网络训练→特定场景微调；大型数据集RoboMIND V2.0（真实数据+ArtVIP仿真数据）；在天工2.0人形机器人、UR、Franka等本体上验证 |
|  2025 |  极佳科技  | [SwiftVLA](https://arxiv.org/pdf/2512.00903)  |  VLM  |  4D transformer（StreamVGGT）实现从2D图像提取4D特征（2D+4D输入），以此来增强VLM（SmolVLM-0.5B）的特征，真机实验比7倍更大的VLA模型（π0）性能相当（真机成功率为0.76，π0为 0.48，LIBERO成功率为95.1%），在边端设备（NVIDIA Jetson Orin）上快18倍（推理速度为0.167s=5.98HZ），内存消耗少12倍 |
|  2025 |  复旦大学  | [ProphRL](https://arxiv.org/pdf/2511.20633)  |  VLA+RL+World Model  | 提出了一个世界模型（Prophet）作为模拟器来训练VLA，方法采用VLA+在线RL；在AgiBot、DROID、LIBERO 和 BRIDGE 等benchmark上为各类 VLA 模型（VLA-adapter-0.5B, Pi0.5-3B,  OpenVLA-OFT-7B）带来 5–17% 的SR提升，在真实机器中取得 24–30% 的成功率提升 | 
|  2025 |  复旦大学  | [WholeBodyVLA](https://arxiv.org/pdf/2512.11047)  |  VLM+RL | 统一框架实现全身控制（下肢移动+上肢捉取）。LAM （Latent Action Model）采用分段训练的方式，用真实机器人操作数据集（AgiBot World）+带有“移动-操作”的视频进行预训练，学习locomotion–manipulation skill。而loco–manipulation–oriented (LMO) RL policy则用于实现稳定的移动（如，前进、转弯、下蹲） |
|  2025 |  阿里达摩院  | [RynnVLA-002](https://arxiv.org/pdf/2511.17502)  |  Action World Model  | 统一框架实现（VLA+World Model）；“双向增强” 逻辑：世界模型通过学习物理动态（预测/生成未来图像），可优化 VLA 模型的动作生成精度；而 VLA 模型的对视觉的理解，能提升世界模型的场景预测保真度；在LIBERO的成功率达97.4% | 
|  2025 |  清华大学  | [X-VLA](https://arxiv.org/pdf/2510.10274?)  |  双系统  |  通过可学习的Soft-Prompt（编码硬件特性） 嵌入让模型“理解”不同机器人和环境的差异；0.9B参数量(LIBERO上平均SR为98.1%)；实现120分钟无辅助自主叠衣服 |
|  2025 | National University of Singapore | [VLA-4D](https://arxiv.org/pdf/2511.17199)  |  4D(VGGT+时间维度)+VLM(Qwen2.5-VL-7B,vision encoder + LLM)  | 4D感知视觉表征，视觉特征（三维位置信息）+一维时间信息嵌入；时空动作表征（空间动作表征拓展了时序信息维度） |
|  2025 |  University of Maryland  | [TraceGen](https://arxiv.org/pdf/2511.21690)  |  DINO（几何特征）+SigLIP（语义特征）+T5（编码指令）+flow-based model  | 引入“3D轨迹空间”（world model/VGGT），将视频转换为3D轨迹；把预测的3D轨迹，通过逆运动学转换成机器人的关节运动指令，直接驱动机器人执行。但真实机器人上的成功率仍然只有67.5% |
| 2025 |  上海交大  | [Evo-1](https://arxiv.org/pdf/2511.04555)   |  VLM+cross-modulated diffusion transformer  | VLM为Internvl3(InternViT-300M+Qwen2.5-0.5B);采用两阶段训练（freeze VLM训练action expert+ full-scale fine-tuning）；仅0.77B参数在LIBERO上获取94.8%，MetaWorld（80.6%，PI0 47.9%）和RoboTwin（37.8%，PI0 30.9%）| 
|  2025 |  HKUST-GZ  | [Spatial Forcing](https://arxiv.org/pdf/2510.12276)  |  VGGT+PI0（VLM+action expert） | 隐式地给VLA赋予3D感知能力；在中间层（相对较深但非最深）通过最大化余弦相似度，强制视觉token与3D特征的对齐；LIBERO benchmark4个任务的平均成功率>98% |
|  2025 |  上海交大  | [Evo-0](https://arxiv.org/pdf/2507.00416?)  |  VGGT+PI0（VLM+action expert）  | 将VGGT的几何特征与VLM视觉特征通过融合模块（lightweight fusion model）进行融合；在真机捉取实验带来的提升28.53%->57.41% |
|  2025 |  北京大学  | [EvoVLA](https://arxiv.org/pdf/2511.16166)  |  OpenVLA-OFT（ViT+LLM）+自监督RL  | 三个组件：RL（阶段对齐奖励、基于位姿的物体探索）+长时程记忆模块（可查询的数据库）；解决了VLA模型的stage hallucination（即假装完成了某个任务阶段而获取奖励）；将长程任务（搭桥、堆叠、枣子入杯）的平均准确率提升到69.2% |
|  2025 |  Physical Intelligence   | [PI0.6](https://www.pi.website/download/pistar06.pdf)  |  PI0.5（VLM+action expert）+RL  | VLA通过强化学习在现实部署中实现自我改进;基于经验与校正的优势条件策略强化学习(RECAP) ；VLM模型采用Gemma 3 4B，动作专家860M参数量|
| 2025 |  美团  | [RoboTron-Mani](https://arxiv.org/pdf/2412.07215v1)  | 3D 感知增强（RoboData数据集+3D感知架构） + 基于LLM的多模态融合架构  | 通过引入相机参数矫正及occupancy监督来增强3D空间感知能力 |
|  2025 |  Generalist  | [GEN-0](https://generalistai.com/blog/nov-04-2025-GEN-0)  |   Harmonic Reasoning模型被训练同时推理与action | 27万小时真实物理交互数据训练；（机器人领域）首次发现7B参数量以内模型会出现固化，而超过这个参数量，可展示良好Scaling Laws |
|2025|Dexmal|[Realtime-VLA](https://arxiv.org/pdf/2510.26742)| pi0(VLM+diffusion action expert)|cuda graph+simplified graph+optimized kernels；捉取落笔任务成功率100%；RTX 4090 GPU实现30HZ推理及up to 480HZ动作生成|---|
|  2025 |  University of British Columbia  | [NanoVLA](https://arxiv.org/pdf/2510.25122v1)  |  VLM+action expert | 视觉-语言解耦（后期融合+特征缓存）+长短动作分块+自适应选择骨干网络；首次实现在边缘设备(Jetson Orin Nano)上高效运行VLA（41.6FPS）；|
| 2025 |  Shanghai AI Lab  | [InternVLA-M1](https://arxiv.org/pdf/2510.13778) |  VLM planner+action expert双系统  | VLM是采用了空间数据进行训练的，action expert输出可执行的电机指令 |
|2025|Figure AI |[Helix](https://www.figure.ai/news/helix)| VLM+Transformer；快慢双系统  | 首个能让两台机器人同时协同工作的VLA 模型；控制人形上半身|
|2025|Russia|[AnywhereVLA](https://arxiv.org/pdf/2509.21006)|SmolVLA+传统SLAM导航(Fast-LIVO2)+frontier-based探索|消费级硬件上实时运行VLA；移动机械臂|
|  2025 |  AgiBot-World  | [GO-1](https://arxiv.org/pdf/2503.06669?)  |  VLM+Action Expert  | AgiBot World：5个场景下217个task对应的一百万条真实机器人轨迹；通过 latent action representations来提升机器人数据的利用； |
|  2025 |  Physical Intelligence  | [PI0.5](https://openreview.net/pdf?id=vlhoswksBO)  |  PI0+PI-FAST+Hi Robot+多源异构数据  | 多源异构数据联合训练+序列建模统一模态+层次规划推理；首个实现长期及灵巧机械臂操作|
|  2025 |  NVIDIA  | [GR00T N1.5](https://research.nvidia.com/labs/gear/gr00t-n1_5/)  |  双系统； NVIDIA Eagle2.5 VLM + Diffusion Transformer  | VLM在微调和预训练的时候都frozen |
|  2025 |  NVIDIA  | [GR00T N1](https://arxiv.org/pdf/2503.14734)  |  双系统；VLM(NVIDIA Eagle-2 VLM)+flow-matching训练的Diffusion Transformer  |  heterogeneous training data |
|  2025 |  KAIST  | [LAPA](https://arxiv.org/pdf/2410.11758?)  |  VLM  | 首个通过无监督学习（没有真值机器人action label）来训练VLA模型的方法 |
|  2025 |  美的  | [DexVLA](https://arxiv.org/pdf/2502.05855)  |  VLM+diffusion  | 1B参数量的diffusion expert with multi-head架构，实现不同实体形态的学习；三阶段的分离式训练策略；|
|  2025 |  美的  | [DiVLA](https://openreview.net/pdf?id=VdwdU81Uzy)  |  VLM+autoregressive+diffusion  | autoregressive进行推理，而diffusion进行动作生成以控制机器人 |
|  2025 |  上海AI Lab与北京人形  | [TinyVLA](https://arxiv.org/pdf/2409.12514)  |  ViT+LLM  | 在OpenVLA基础上引入轻量VLM模型以及diffusion policy decoder | 
|  2025 |  Stanford  | [OpenVLA-OFT/OpenVLA-OFT+](https://arxiv.org/pdf/2502.19645)  |  ViT+LLM  | 在OpenVLA基础上引入了并行解码、action chunking、连续的动作表示、简单的L1回归作为训练目标；其中OpenVLA-OFT+则是在SigLIP和DINOv2之间插入了FiLM |
|  2025 |  Physical Intelligence  | [Hi Robot](https://arxiv.org/pdf/2502.19417)  |  PI0+快慢双系统（VLM+VLA）  | 分层交互式机器人学习系，可以执行高层推理与底层任务执行 |
|  2025 |  Physical Intelligence  | [PI0-Fast/π₀-FAST](https://arxiv.org/pdf/2501.09747)  |  PI0+频率空间action Tokenization | 探索VLA训练的action representation；通过频域对动作序列的Token化，将训练时间减少5倍 |
|  2024 |  Physical Intelligence  | [π0/PI0](https://arxiv.org/pdf/2410.24164?)  |  VLM+action expert（diffusion）  | 通才模型（generalist model）；预训练+task-specific微调策略 |
|  2024 |  Stanford  | [OpenVLA](https://arxiv.org/pdf/2406.09246?)  |  SigLIP与DNIO-v2作为视觉编码器，大语言模型（LLaMA2-7B）作为高层推理| 首个全面开源的通用 VLA 模型，结合多模态编码与大语言模型架构；首次展示了通过低秩适应（LoRA）和模型量化等计算高效的微调方法，实现降低计算成本且不影响成功率 |
|  2024 |  UC Berkeley  | [Octo](https://arxiv.org/pdf/2405.12213)  |  Transformer  | 采用diffusion作为连续动作生成；基于Open x-embodiment训练的大型架构；通用机器人模型的探索|
|  2024 |  字节  | [GR-2](https://arxiv.org/pdf/2410.06158?)  | GPT-style 视频生成模型   | GR-1的晋级版，用了更多、更diversity的数据来预训练 |
|  2024 |  字节  | [GR-1](https://arxiv.org/pdf/2312.13139)  |  GPT-style 视频生成模型  | 通过GPT式生成模型的预训练（video generative pre-training model），结合机器人数据进行微调 |
|  2024 |  Stanford  | [ReKep](https://arxiv.org/pdf/2409.01652)  |  ViT+VLM  | DINOv2提取3D关键点，VLM通过指令与图像生成关键点与空间的约束，通过求解优化获取机器人末端执行轨迹 |
|  2023 |  Google DeepMind  | [RT-2](https://robotics-transformer2.github.io/assets/rt2.pdf)  |  VLM  | 正式提出VLA概念；采用VLM作为骨架；Internet-scale预训练VLM模型在机器人控制上展示良好的泛化性及语义推理；将action也表达成文本token的形式 |
|2023|Stanford|[ALOHA/ACT](https://arxiv.org/pdf/2304.13705)|CVAE+Transformer|动作分块；用低成本平台实现精细操作,如线扎带、乒乓球|
|  2023 |  Stanford  | [VoxPoser](https://arxiv.org/pdf/2307.05973)  |  LLM+VLM  | LLM进行代码生成驱动机器人完整操作任务，VLM获取3D价值地图获取实现任务的轨迹规划 |
|  2023 |  Google  | [SayCan](https://proceedings.mlr.press/v205/ichter23a/ichter23a.pdf)  |  LLM  | 探索如何利用 LLM 实现对机器人动作的控制,通过预定义的运动（motion primitives）来与环境进行交互 |
|2023|Google DeepMind|[RT-1](https://arxiv.org/pdf/2212.06817)|EfficientNet+Transformer|VLA任务首次用到实际机械臂|


~~~
PS: VLA的方法实在太多了，后续看到有意思的工作会及时更新此表格，但方法解读实在没办法都整理，只能囫囵吞枣，错漏之处，欢迎评论区指出🙏
~~~

## VLA常用的数据集

<!-- |---|`arXiv`|---|---|---| -->
<!-- [![Github stars](https://img.shields.io/github/stars/***.svg)]() -->

| Year | Venue | Paper Title | Repository | Note |
|:----:|:-----:| ----------- |:----------:|:----:|
|2026|`arXiv`|[Being-H0.5: Scaling Human-Centric Robot Learning for Cross-Embodiment Generalization](https://research.beingbeyond.com/projects/being-h05/being-h05.pdf)| [![Github stars](https://img.shields.io/github/stars/BeingBeyond/Being-H.svg)](https://github.com/BeingBeyond/Being-H)|[website](https://research.beingbeyond.com/being-h05)<br>UniHand 2.0超过 14,000 小时 的机器人操作数据与 16,000 小时 的人类视频数据,|
|2025|`arXiv`|[RoboWheel: A Data Engine from Real-World Human Demonstrations for Cross-Embodiment Robotic Learning](https://arxiv.org/pdf/2512.02729)|[![Github stars](https://img.shields.io/github/stars/zhangyuhong01/Robowheel-Toolkits.svg)](https://github.com/zhangyuhong01/Robowheel-Toolkits)<br>[website](https://zhangyuhong01.github.io/Robowheel)<br>[dataset](https://huggingface.co/datasets/HORA-DB/HORA)|数据引擎，将普通单目 RGB/RGB-D 相机拍摄的人类手-物交互（HOI）视频，转化为适用于工业机械臂、灵巧手、人形机器人等不同形态设备的训练数据，无需复杂硬件即可实现媲美遥操作的训练效果。包含了15万条轨迹的机器人训练数据|
|2025|`arXiv`|[Galaxea open-world dataset and g0 dual-system vla model](https://arxiv.org/pdf/2509.00576)|[![Github stars](https://img.shields.io/github/stars/OpenGalaxea/GalaxeaVLA.svg)](https://github.com/OpenGalaxea/GalaxeaVLA)|[website](https://opengalaxea.github.io/GalaxeaVLA/)<br>500+小时真实机器人数据|
|2025|`RSS`|[Robomind: Benchmark on multi-embodiment intelligence normative data for robot manipulation](https://arxiv.org/pdf/2412.13877)|[![Github stars](https://img.shields.io/github/stars/x-humanoid-robomind/x-humanoid-robomind.github.io.svg)](https://github.com/x-humanoid-robomind/x-humanoid-robomind.github.io)|[website](https://x-humanoid-robomind.github.io/)|
|2025|`arXiv`|[InternData-A1: Pioneering High-Fidelity Synthetic Data for Pre-training Generalist Policy](https://arxiv.org/pdf/2511.16651)|[huggingface](https://huggingface.co/datasets/InternRobotics/InternData-A1)|[website](https://internrobotics.github.io/interndata-a1.github.io/)|
|2025|`arXiv`|[Robotwin 2.0: A scalable data generator and benchmark with strong domain randomization for robust bimanual robotic manipulation](https://arxiv.org/pdf/2506.18088)|---|[website](https://robotwin-platform.github.io/)|
|2025|`IROS`|[Agibot world colosseo: A large-scale manipulation platform for scalable and intelligent embodied systems](https://arxiv.org/pdf/2503.06669)|[![Github stars](https://img.shields.io/github/stars/OpenDriveLab/AgiBot-World.svg)](https://github.com/OpenDriveLab/AgiBot-World)|[website](https://agibot-world.com/) <br> AgiBot World dataset|
|2025|`RSS`|[Robomind: Benchmark on multi-embodiment intelligence normative data for robot manipulation](https://arxiv.org/pdf/2412.13877)|[![Github stars](https://img.shields.io/github/stars/x-humanoid-robomind/x-humanoid-robomind.github.io.svg)](https://github.com/x-humanoid-robomind/x-humanoid-robomind.github.io)|[website](https://x-humanoid-robomind.github.io/)|
|2025|`ICRA`|[Dexmimicgen: Automated data generation for bimanual dexterous manipulation via imitation learning](https://arxiv.org/pdf/2410.24185)|[![Github stars](https://img.shields.io/github/stars/NVlabs/dexmimicgen.svg)](https://github.com/NVlabs/dexmimicgen/)|[website](https://dexmimicgen.github.io/)<br>DexMimicGen|
|2024|`ICRA`|[Rh20t: A comprehensive robotic dataset for learning diverse skills in one-shot](https://rh20t.github.io/static/RH20T_paper_compressed.pdf)|[![Github stars](https://img.shields.io/github/stars/rh20t/rh20t_api.svg)](https://github.com/rh20t/rh20t_api)|[website](https://rh20t.github.io/)| 
|2024|`RSS`|[Robocasa: Large-scale simulation of everyday tasks for generalist robots](https://arxiv.org/pdf/2406.02523)|[![Github stars](https://img.shields.io/github/stars/robocasa/robocasa.svg)](https://github.com/robocasa/robocasa)|[website](https://robocasa.ai/)|
|2024|`RSS`|[Droid: A large-scale in-the-wild robot manipulation dataset](https://arxiv.org/pdf/2403.12945)|---|[website](https://droid-dataset.github.io/)|
|2024|`ICRA`|[Roboagent: Generalization and efficiency in robot manipulation via semantic augmentations and action chunking](https://arxiv.org/pdf/2309.01918)|[![Github stars](https://img.shields.io/github/stars/robopen/roboagent.svg)](https://github.com/robopen/roboagent/)|[website](https://robopen.github.io/)|
|2023|`NIPS`|[Libero: Benchmarking knowledge transfer for lifelong robot learning](https://proceedings.neurips.cc/paper_files/paper/2023/file/8c3c666820ea055a77726d66fc7d447f-Paper-Datasets_and_Benchmarks.pdf)|---|[website](https://libero-project.github.io/)<br>LIBERO|
|2023|`CoRL`|[Bridgedata v2: A dataset for robot learning at scale](https://proceedings.mlr.press/v229/walke23a/walke23a.pdf)|[![Github stars](https://img.shields.io/github/stars/rail-berkeley/bridge_data_v2.svg)](https://github.com/rail-berkeley/bridge_data_v2)|[website](https://rail-berkeley.github.io/bridgedata/)<br>WidowX|
|2023|`CoRL`|[Open x-embodiment: Robotic learning datasets and rt-x models](https://arxiv.org/pdf/2310.08864)|[![Github stars](https://img.shields.io/github/stars/google-deepmind/open_x_embodiment.svg)](https://github.com/google-deepmind/open_x_embodiment)|[website](https://robotics-transformer-x.github.io/)|
|2023|`CoRL`|[Rt-2: Vision-language-action models transfer web knowledge to robotic control](https://robotics-transformer2.github.io/assets/rt2.pdf)|---|[Website](https://robotics-transformer2.github.io/)|
|2022|`RAL`|[Calvin: A benchmark for language-conditioned policy learning for long-horizon robot manipulation tasks](https://arxiv.org/pdf/2112.03227)|[![Github stars](https://img.shields.io/github/stars/mees/calvin.svg)](https://github.com/mees/calvin)|[website](http://calvin.cs.uni-freiburg.de/)|
|2022|`arXiv`|[Rt-1: Robotics transformer for real-world control at scale](https://arxiv.org/pdf/2212.06817)|[![Github stars](https://img.shields.io/github/stars/google-research/robotics_transformer.svg)](https://github.com/google-research/robotics_transformer)|[website](https://robotics-transformer1.github.io/) <br> Google robot|
|2022|`CoRL`|[BC-Z: Zero-shot task generalization with robotic imitation learning](https://proceedings.mlr.press/v164/jang22a/jang22a.pdf)|---|[website](https://sites.google.com/view/bc-z/home)|
|2022|`RSS`|[Bridge data: Boosting generalization of robotic skills with cross-domain datasets](https://arxiv.org/pdf/2109.13396)|[![Github stars](https://img.shields.io/github/stars/yanlai00/bridge_data_imitation_learning.svg)](https://github.com/yanlai00/bridge_data_imitation_learning) <br> [![Github stars](https://img.shields.io/github/stars/yanlai00/bridge_data_robot_infra.svg)](https://github.com/yanlai00/bridge_data_robot_infra) |[website](https://sites.google.com/view/bridgedata) <br> Google robot|
|2025|`CoRL`|[Meta-world: A benchmark and evaluation for multi-task and meta reinforcement learning](https://proceedings.mlr.press/v100/yu20a/yu20a.pdf)|[![Github stars](https://img.shields.io/github/stars/Farama-Foundation/Metaworld.svg)](https://github.com/Farama-Foundation/Metaworld)|[website](https://metaworld.farama.org/)|
|2019|`CoRL`|[Robonet: Large-scale multi-robot learning](https://arxiv.org/pdf/1910.11215)|---|[website](https://www.robonet.wiki/)|


<!-- |---|`arXiv`|---|---|---| -->
<!-- [![Github stars](https://img.shields.io/github/stars/***.svg)]() -->

