# Car-following simulation

这是一个用于课堂讲解的单车道跟驰仿真。车辆使用 IDM（Intelligent Driver Model）决定加速度；道路中的临时限速区会产生减速、排队和向上游传播的拥堵波，最终用时空图展示。

## 创建环境

建议使用 Python 3.11 或更新版本：

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## 运行

```bash
python main.py --experiment 1
```

程序会输出车辆总数和平均流入率，并显示时空图。轨迹颜色表示车辆速度，白色虚线是 `-16 km/h` 的参考拥堵波速度。

四个实验组合了两种流入方式和两种瓶颈持续时间：

| 实验 | 车辆流入 | 瓶颈持续时间 |
| --- | --- | --- |
| 1 | 固定间隔 | 短（100–200 s） |
| 2 | 随机间隔 | 短（100–200 s） |
| 3 | 固定间隔 | 持续到仿真结束 |
| 4 | 随机间隔 | 持续到仿真结束 |

保存图片而不打开窗口：

```bash
python main.py --experiment 2 --output results/experiment-2.png --no-show
```

## 课堂讲解顺序

1. `config.py`：道路、车辆、IDM、瓶颈和四种实验参数。
2. `vehicle.py`：所有车辆共有的状态更新与安全约束。
3. `vehicle_idm.py`：期望间距与 IDM 加速度公式。
4. `simulator.py`：每个时间步中的“生成车辆 → 道路检查 → 更新状态 → 记录”。
5. `plotting.py`：把每辆车的历史轨迹转换成速度着色的时空图。

`vehicle_knn.py` 和 `vehicle_llm.py` 是课堂扩展接口，展示如何把 IDM 替换成数据驱动或 LLM 驱动的策略；它们尚未实现训练数据或决策服务，因此默认仿真不会调用。

## IDM 的核心关系

车辆会综合自由行驶目标和与前车的交互来确定加速度：

```text
期望动态间距 s* = 最小间距 + 速度 × 安全时距 + 相对速度修正
加速度 = 最大加速度 × [1 - (当前速度 / 期望速度)^4 - (s* / 实际间距)^2]
```

仿真采用固定时间步的数值更新，并额外限制速度非负、加速度范围和最小安全间距。
