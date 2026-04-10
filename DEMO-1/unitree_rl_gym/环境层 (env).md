# vec_env.py
## ABC (Abstract Base Class) 抽象基类模式
抽象基类 （Abstract Base Class, ABC）是 Python 中的一种设计模式，用于定义 接口规范 。
### 抽象基类的特点
1. 不能实例化 ：不能直接创建抽象基类的对象
2. 定义接口 ：规定子类必须实现哪些方法
3. 强制约束 ：子类未实现抽象方法会报错

## 🔍 Python 中的 ABC 实现机制
### 使用 @abstractmethod 装饰器
```python
from abc import ABC, abstractmethod

class MyClass(ABC):
    @abstractmethod
    def required_method(self):
        """这个方法必须在子类中实现"""
        pass
    
    @abstractmethod
    def another_required_method(self, param):
        """这个方法也必须在子类中实现"""
        pass
```
在 rsl_rl 中的应用
### 优势
1. 接口一致性 ：所有环境实现都必须有 step() 和 reset() 方法
2. 类型安全 ：算法可以安全地调用这些方法
3. 错误提示 ：如果环境未实现接口，会立即得到明确的错误信息

ABC 的核心价值 ：

- ✅ 强制接口实现 ：确保子类具备必要的方法
- ✅ 提高代码可靠性 ：在实例化时就发现接口缺失
- ✅ 清晰的文档 ：明确告诉开发者需要实现什么
- ✅ 类型安全 ：支持类型检查和 IDE 提示

### 环境配置参数
num_envs - 并行环境数量

- 作用 ：同时运行多个环境实例
- 典型值 ：4096（大规模并行）
- 优势 ：
  - 提高数据收集效率
  - 减少环境切换开销
  - 稳定训练过程
num_obs - 观测维度

- 作用 ：Actor 网络的输入大小
- 示例 ：机器人控制中可能包含
  - 关节位置（12维）
  - 关节速度（12维）
  - 姿态（4维）
  - 速度（3维）
  - 总计：48维
num_privileged_obs - 特权观测维度

- 作用 ：Critic 网络的输入大小
- 特点 ：包含额外信息（可能包含环境参数、目标位置等）
- 设计目的 ：实现 State-Based Value Function
  - Actor：只能看到部分信息（模拟真实场景）
  - Critic：可以看到完整状态（用于价值估计）
num_actions - 动作维度

- 作用 ：Actor 网络的输出大小
- 示例 ：12个关节电机的力矩控制
max_episode_length - 最大回合长度

- 作用 ：限制单个 episode 的最大步数
- 用途 ：防止 episode 过长，定期重置环境

### 环境状态缓冲区
obs_buf - 观测缓冲区
```python
# 形状: [num_envs, num_obs]
# 示例: [4096, 48] 表示4096个环境，每个环境48维观测
self.obs_buf = torch.zeros(num_envs, num_obs, device=device)
```
privileged_obs_buf - 特权观测缓冲区
```python
# 形状: [num_envs, num_privileged_obs]
# 示例: [4096, 52] 表示4096个环境，每个环境52维特权观测
self.privileged_obs_buf = torch.zeros(num_envs, num_privileged_obs, device=device)
```
rew_buf - 奖励缓冲区
```python
# 形状: [num_envs]
# 存储每个环境的即时奖励
self.rew_buf = torch.zeros(num_envs, device=device)
```
reset_buf - 重置标志缓冲区
```python
# 形状: [num_envs]
# 布尔类型，True表示对应环境需要重置
self.reset_buf = torch.zeros(num_envs, device=device, dtype=torch.bool)
```
episode_length_buf - 回合长度缓冲区
```python
# 形状: [num_envs]
# 记录每个环境当前episode的步数
self.episode_length_buf = torch.zeros(num_envs, device=device)
```
extras - 额外信息字典
```python
# 存储额外信息
self.extras = {
    'time_outs': torch.zeros(num_envs, device=device),  # 超时标志
    'episode': {'rew': [], 'len': []}  # 回合统计信息
}
```
device - 设备类型
```python
# 指定张量存储的设备
self.device = torch.device('cuda')  # 或 'cpu'
# 确保所有缓冲区在同一设备上以获得最佳性能
```
### 抽象方法详解
#### step() 方法
```python
@abstractmethod

def step(self, actions: torch.Tensor) -> Tuple[torch.Tensor, Union[torch.Tensor, None], torch.Tensor, torch.Tensor, dict]:

"""执行动作并返回环境步进结果

参数:

actions: 动作张量，形状为 [num_envs, num_actions]

返回:

next_observations: 下一时刻的观测，形状 [num_envs, num_obs]

privileged_observations: 特权观测（可选），形状 [num_envs, num_privileged_obs] 或 None

rewards: 奖励值，形状 [num_envs]

dones: 终止标志，形状 [num_envs]，True表示episode结束

infos: 额外信息字典，应包含'time_outs'等信息

"""

pass
```

返回值类型 Tuple[...]

```python
Tuple[
    torch.Tensor,                    # next_observations
    Union[torch.Tensor, None],       # privileged_observations
    torch.Tensor,                    # rewards
    torch.Tensor,                    # dones
    dict                             # infos
]
```
#### reset() 方法
```python

@abstractmethod
def reset(self, env_ids: Union[list, torch.Tensor]):

"""重置指定的环境

参数:

env_ids: 要重置的环境ID列表或张量，可以是单个或多个环境

"""

pass
```
#### get_observations() 方法
```python
@abstractmethod

def get_observations(self) -> torch.Tensor:

"""获取当前观测

返回:

当前观测张量，形状为 [num_envs, num_obs]

"""

pass
```
#### get_privileged_observations()
```python
@abstractmethod

def get_privileged_observations(self) -> Union[torch.Tensor, None]:

"""获取特权观测（Critic的输入）

返回:

特权观测张量，形状为 [num_envs, num_privileged_obs]

如果没有特权观测，返回 None

"""

pass
```


```
Actor (Policy)          Critic (Value)
     │                        │
     │ 只能看到部分信息       │ 可以看到完整信息
     │ (obs_buf)              │ (privileged_obs_buf)
     ▼                        ▼
  观测空间较小              观测空间较大
  模拟真实场景              用于价值估计
```

特权观测 是强化学习中的一种高级技术，用于区分 Actor（策略网络） 和 Critic（价值网络） 的输入信息。
区分 Actor 和 Critic 的信息访问权限：

- ✅ Actor ：基于部分观测决策（模拟真实场景）
- ✅ Critic ：基于完整信息评估价值（提升稳定性）
```
┌─────────────────────────────────────────────────────────┐
│                    环境真实状态                          │
│  (包含所有信息：位置、速度、物理参数、目标等)             │
└─────────────────────────────────────────────────────────┘
                      │
        ┌─────────────┴─────────────┐
        │                           │
        ▼                           ▼
┌─────────────────┐       ┌─────────────────┐
│  Actor (Policy) │       │  Critic (Value) │
│  (策略网络)      │        │   (价值网络)      │
│                 │       │                 │
│ 只能看到部分信息   │       │  可以看到完整信息 │
│ (非特权观测)      │       │  (特权观测)      │
│ 例如: 48维       │       │  例如: 52维      │
└────────┬────────┘       └────────┬────────┘
         │                          │
         ▼                          ▼
    选择动作                    评估状态价值
```
非特权观测（Normal Observations）
特点 ：

- ✅ 只包含 可见的、可测量的 信息
- ✅ 模拟真实世界的 部分可观测性
- ✅ Actor 只能基于当前观测决策
特权观测（Privileged Observations）
特点 ：

- ✅ 包含 额外的环境参数和全局信息
- ✅ 可能包含 环境配置参数
- ✅ Critic 可以基于完整信息评估价值

```
# 特权观测可能导致过拟合
# 训练时表现好，但实际部署时性能下降

# 建议：
# 1. 先用非特权观测训练
# 2. 再引入特权观测微调
# 3. 验证在真实场景的泛化能力
```