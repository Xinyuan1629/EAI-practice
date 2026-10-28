# 具身智能探索记录

从机器人仿真环境复现，到独立评估，再到跨机器人迁移对照。这里展示我在 2026 年 10 月完成的三个项目；每项都区分了**原有开源实现**、**我新增的工作**和**结果的适用范围**。

| 项目 | 我完成的工作 | 可核对的结果 |
|---|---|---|
| [PushCube PPO](#1-maniskill-pushcube-ppo) | 官方基线复现、固定种子独立评估、失败轨迹分析 | 100 回合中 90 回合曾成功，78 回合最终成功 |
| [Franka → UR5 ACT](#2-franka--ur5-示范迁移) | 双机器人数据管线、状态 ACT、12 组对照与 2 组诊断 | 未观察到同预算正迁移；记录了种子差异 |
| [ContactExplorer](#3-contactexplorer-灵巧操作复现) | 论文和源码解读、单任务训练与对照、接触及扰动评估 | 单训练种子的固定策略评估；不等于论文全量复现 |

## 1. ManiSkill PushCube PPO

使用 [ManiSkill 官方 PPO](https://github.com/mani-skill/ManiSkill/tree/main/examples/baselines/ppo) 单文件基线，在一张 RTX 4090 上以 seed 1 训练 PushCube-v1，实际采集 **1,966,080 条 transition**。我另外编写固定测试协议：100 个种子、每回合 50 步、确定性动作，分别计算“曾达到目标”和“最后仍在目标内”。

训练前两项均为 **0/100**；训练后分别为 **90/100** 和 **78/100**。12 个回合到达后离开，轨迹中存在推过目标的例子。训练成功即重置、评估继续运行 50 步是一个待检验的解释，尚未做因果对照。

[详细结果](evidence/pushcube-ppo-results.md) · [学习与失败轨迹图](evidence/ppo-learning-and-failure.png)

## 2. Franka → UR5 示范迁移

构建 Panda 与 UR5 推杆的 PushCube 仿真、示范采集和重放流程，取得 **1200 条 Panda** 与 **20 条 UR5** 成功轨迹；抽样并集中的 1194 条重放轨迹全部通过接受检查。我实现精简的状态观测 ACT，并按 3 个训练种子 × 4 个源数据规模完成 **12 次正式运行**，每次在 200 个独立初态上测试，另做 2 次诊断。

同预算下未观察到正迁移：UR5-only 三个种子的最终成功率是 **97%、0%、46.5%**，混入 100/500/1000 条源轨迹的各组均为 **0%**。目标样本曝光量与收敛稳定性影响解释；不能把这个结果推广为“跨机器人示范无效”。

[逐种子结果与实验限制](evidence/franka-ur5-act-results.md) · [对照图](evidence/act-comparison.png)

## 3. ContactExplorer 灵巧操作复现

基于 [ContactExplorer 官方仓库](https://github.com/ruoyiqiao/ContactExplorer) 和[论文](https://arxiv.org/abs/2603.10971)，阅读接触覆盖计数、能量引导奖励与 PPO 的实现，选取 LEAP 手 CubeInBox 任务。跑通训练、容量检查和独立评估，随后分析 ContactExplorer 与移除探索奖励的 PPO 对照，并继续检查接触行为和摩擦扰动。

一个训练种子的固定策略，在 1250 次更新处的两个 nominal 评估种子分别为 **373/384（97.14%）** 和 **359/384（93.49%）**；2000 次更新处为 **333/384（86.72%）** 和 **312/384（81.25%）**。对应 baseline 在 2000 次更新处均为 **0/384**。这些评估种子重复测试的是固定策略，不能当作独立训练种子；也不代表论文的多任务、多种子或真实机器人复现。

[固定策略评估明细](evidence/contactexplorer-fixed-policy-evaluation.md)

## 接下来

- 让 UR5-only ACT 基线在多个训练种子上更稳定，并控制目标数据曝光量后重做迁移对照。
- 检验 PPO 成功后继续控制是否导致目标丢失。
- 为 ContactExplorer 增加独立训练种子，分离 checkpoint 选择与评估差异。

## 代码与来源

本仓库是研究入口和证据摘要。完整项目分别见 [PushCube PPO](https://github.com/Xinyuan1629/pushcube-ppo-reproduction)、[Franka → UR5 ACT](https://github.com/Xinyuan1629/franka-ur5-pushcube-act) 和 [ContactExplorer 学习与评估记录](https://github.com/Xinyuan1629/contactexplorer-study)。ContactExplorer 的算法与主体代码属于原作者；ManiSkill PPO 基线也属于官方项目。本仓库仅陈述我的复现、评估、诊断和实验记录。
