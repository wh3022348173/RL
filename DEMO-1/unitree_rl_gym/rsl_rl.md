```text
rsl_rl-main/
├── rsl_rl/
│   ├── algorithms/
│   │   ├── ppo.py                      # PPO算法的核心实现，负责策略更新、损失计算、自适应学习率等
│   │   └── distillation.py             # 策略蒸馏算法实现，用于知识迁移和模型压缩
│   ├── env/
│   │   └── vec_env.py                  # 并行环境管理，统一-reset/step接口，批量采样
│   ├── modules/
│   │   ├── actor_critic.py             # Actor-Critic神经网络结构(MLP)，策略与价值评估
│   │   ├── actor_critic_recurrent.py   # 带RNN的Actor-Critic，支持时序依赖
│   │   ├── normalizer.py               # 观测/奖励归一化模块，提升训练稳定性
│   │   └── student_teacher.py          # 学生-教师网络结构，支持策略蒸馏
│   ├── networks/
│   │   └── memory.py                   # RNN记忆模块，管理隐藏状态，支持LSTM/GRU
│   ├── runners/
│   │   └── on_policy_runner.py         # 训练主流程编排，调度采样、更新、日志、保存等
│   ├── storage/
│   │   └── rollout_storage.py          # 采样数据缓冲区，支持GAE、批量采样、数据整理
│   ├── utils/
│   │   ├── utils.py                    # 通用工具函数
│   │   ├── wandb_utils.py              # Wandb日志工具
│   │   ├── neptune_utils.py            # Neptune日志工具
│   │   └── __init__.py                 # 包初始化文件
│   └── config/
│       └── dummy_config.yaml           # 示例配置文件，定义算法、网络、训练等参数
├── scripts/
│   ├── train.py                        # 训练入口脚本，加载配置并启动训练
│   └── evaluate.py                     # 评估脚本，测试训练好的模型
├── README.md                           # 项目说明文档，介绍用法、结构和依赖
└── requirements.txt                    # Python依赖包列表
```
### 1. 算法层 (algorithms/)
- ppo.py : PPO 算法实现
  - 使用 clipped surrogate loss 防止策略更新过大
  - 支持自适应学习率（基于 KL 散度）
  - 支持值函数裁剪（clipped value loss）
  - 包含熵正则化项
### 2. 模型层 (modules/)
- actor_critic.py : 基础 Actor-Critic 网络
- Actor : 从观测输出动作（高斯分布）
- Critic : 从观测估计价值函数
- 支持可配置的隐藏层维度和激活函数
- actor_critic_recurrent.py : 带 RNN 的版本
- 使用 LSTM/GRU 处理时序信息
- 适用于部分可观测环境
### 3. 存储层 (storage/)
- rollout_storage.py : 回合存储器
  - 存储 rollout 过程中的所有转换
  - 计算 GAE（Generalized Advantage Estimation）
  - 支持批量和 RNN 的 mini-batch 生成器
### 4. 环境层 (env/)
- vec_env.py : 向量化环境接口
  - 抽象基类，定义环境必须实现的接口
  - 支持多个并行环境
### 5. 运行器层 (runners/)
- on_policy_runner.py : on-policy 训练运行器
  - 协调环境交互、数据收集和模型训练
  - 提供日志记录和模型保存功能
### 6. 工具层 (utils/)
- utils.py : 工具函数
  - split_and_pad_trajectories() : 分割和填充轨迹
  - unpad_trajectories() : 去除填充