# 逐际动力WF_TRON1A机器人控制器训练

LimX Dynamics WF_TRON1A 强化学习控制器训练项目

本项目专注于使用强化学习（RL）训练 WF_TRON1A（轮腿机器人）的控制器，解决在机器人实际开发仿真过程中遇到的  
机器人无法完全静止站立，存在缓慢旋转和移动的问题。通过重新训练控制器使其能够在mujuco仿真环境完全静止站立（零速度、稳定平衡）。


## 项目简介

本仓库是基于 LimX Dynamics 官方仿真与部署框架的强化学习训练项目。主要目标是通过 MuJoCo 仿真环境 + RL 算法，训练出高性能的站立控制器，实现机器人从初始蹲姿平稳站立并保持完全静止。

**核心功能**：
- MuJoCo 物理仿真环境（`tron1-mujoco-sim`）
- RL 控制器部署（`tron1-rl-deploy-python`）
- RL 控制器训练（`tron1-rl-isaaclab`）
- 支持 WF_TRON1A 轮腿机器人
- 重点训练**静止站立**策略（零速度平衡控制）


## 问题分析与解决效果  
### 问题描述
在WF-TRON1A机器人开发过程中，遇到机器人在仿真环境中无法静止站立的问题，对后续建图导航开发带来一定阻碍，具体表现为  
机器人设置速度为0（角速度和线速度）时，仍然发生旋转，并且出现缓慢直线移动，此外机器人的腿部并不对称，这会导致机器人在  
仅有线速度控制指令的情况下仍然无法直线前进。如下图所示：  
<img width="800" height="653" alt="origin_standstill" src="https://github.com/user-attachments/assets/1fa71735-e886-4816-ab63-f3f298911c5e" />

### 问题分析
通过分析原始参数，并进行多轮实验，分析Tensorboard输出曲线，发现可能的原因为：由于原始参数中对机器人静止状态下产生移动的惩罚  
较小，导致控制器训练过程中倾向于通过移动来获得其他方面的奖励，此外针对腿部对称效果的惩罚不足，导致机器人容易出现腿部不对称问题  
通过调整参数并进行多次训练，成功得到了一个能够解决上述问题的控制器

### 解决效果
<img width="800" height="653" alt="best_standstill" src="https://github.com/user-attachments/assets/4e3cac6f-5f9b-4547-94dc-156c29e8e041" />  

可以发现，通过重新训练控制器，机器人成功在原地保持静止，并且腿部处于对称状态不会出现直线前进时发生轻微转向的问题

## 项目启动
- 克隆项目
```bash
git clone https://github.com/F-Morry/limx_rl.git
cd limx_rl
```
- 启动mujuco仿真
```bash
cd limx_rl
python tron1-mujuco-sim simulator.py
```
- 启动控制器
```bash
cd limx_rl
python tron1-rl-deploy-python main.py
```

- 控制器训练
控制器训练还需建立对应的环境，具体操作方式见逐际动力官方控制器训练教程（本项目在Issacsim4.5 Isaaclab2.0环境下训练）

