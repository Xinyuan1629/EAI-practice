# 第一次 ManiSkill3 复现结果

2026-10-09。已完成环境交互、GPU 仿真与渲染、PushCube PPO 训练、独立评估和录像核验。

## 实际环境

- Mac 是操作端，通过 SSH 执行远端命令；远端没有安装 Codex。
- 远端：Ubuntu 24.04，使用一张 RTX 4090（24 GB）。GPU 由 UUID 指定，避免 PyTorch 与 nvidia-smi 排序差异。
- Python 3.12.3 / ManiSkill 3.0.1 / SAPIEN 3.0.3 / PyTorch 2.8.0+cu128 / Gymnasium 1.4.0。
- 远端目录：`/mnt/disk2/xinyuan.liu/maniskill3-reproduction`。
- 版本与源码校验：`results/requirements-linux.lock.txt`、`results/experiment_manifest.json`。
- Mac 的独立环境安装因下载超时未完成；操作远端训练不需要它。

## 已完成的检查

PushCube 随机交互：3 回合、150 条 transition。PickCube 随机交互：2 回合、100 条 transition。随机策略均未成功，数据通路正常。

GPU 检查：16 个并行 PushCube 环境，状态 `(16, 35)`、动作 `(16, 8)`、离屏渲染 `(16, 512, 512, 3)`，状态和奖励均为有限值。

PushCube PPO 使用官方单文件和示例参数：2048 个环境，每次 rollout 20 步，8 个更新 epoch，32 个 minibatch，seed=1。配置请求 2,000,000 步；代码按整批取整，实际完成 `48 × 2048 × 20 = 1,966,080` 条 transition。最后报告约 8,655 SPS，训练过程约 3.8 分钟（含内置评估等开销）。

这是一次训练 seed 的初始复现，不是多 seed 的正式 benchmark。没有使用人类示范或遥操作数据；rollout 来自机器人与仿真环境的交互。

## 独立评估

使用同一组 100 个测试种子 `10001–10100`，完整运行 50 步，确定性动作，GPU 仿真。训练前 `ckpt_1.pt` 与训练后 `final_ckpt.pt` 使用相同协议。

| 指标 | 初始策略 | 训练后策略 |
|---|---:|---:|
| 过程中曾达到目标 | 0/100 | 90/100 |
| 回合结束时仍达到目标 | 0/100 | 78/100 |
| 平均回合回报（normalized_dense） | 3.90 | 37.32 |

策略学到了推方块行为，但还不稳定。90 个曾成功的回合中，12 个在结束时失去成功状态；另外 10 个从未成功。

训练内置评估每次仅 8 回合，后期显示过 100% 成功。它与独立评估的样本数和种子不同，不能直接把那个 100% 当作最终泛化结果。

![训练与失败轨迹](results/learning_and_failure.png)

## 录像与轨迹核验

- [成功例子](results/test_videos/1.mp4)：第 13 步首次成功，结束时方块中心距目标中心约 5.19 cm。
- [到达后又离开](results/test_videos/2.mp4)：第 10 步首次成功，最终距离约 10.38 cm，超过 10 cm 的成功半径。方块仍在桌面上，最终相对目标 XY 偏移约 `[10.07, 2.50] cm`，表现为推过目标。
- 另一个成功录像是 `results/test_videos/0.mp4`。三段录像各 51 帧、30 FPS，是另外 3 个评估回合，不是上述 100 回合的录像。
- 对应状态、动作、奖励和 success 标记在 `results/test_videos/trajectory.h5`，回合种子在 `trajectory.json`。

## 第一个值得理解的代码差异

官方 PPO 默认训练 `partial_reset=True`，成功会触发结束和重置；默认评估 `eval_partial_reset=False`，忽略提前成功的结束标记，继续运行到 50 步。

训练与评估对“成功之后继续控制多久”的要求存在差异。它是“到达后又离开”的候选解释之一，尚未通过对照实验确认因果。先阅读 reset / termination / wrapper 的关系，再决定是否做训练时不提前重置的实验。

结论限于这个仿真任务、状态观测和参数分布；没有进行视觉 RL 或真实机器人实验。

## 文件入口

- [下一次代码阅读](NEXT_SESSION.md)
- [交互检查脚本](inspect_rollout.py)
- [官方 PPO 源码](ppo.upstream.py)
- [本次安装的 PushCube 源码](results/source_snapshot/push_cube.py)
- [训练日志](results/train.log)
- [100 回合评估详情](results/evaluation_final.json)

初始及最终 checkpoint 均已取回 `results/`。TensorBoard 日志与中间 checkpoint 保留在远端 `runs/pushcube_ppo_seed1/`。
