# PaddleX 超参数介绍
PaddleX 暴露了模型迭代中最常修改的参数，方便您在配置文件中或者命令行中进行修改。训练模型的基础配置和高级配置参数如下：

## 1. 基础参数
对于非时序任务模块，基础参数包括`epochs_iters`，`batch_size`，`class_num`，`learning_rate`，相关的参数解释如下：
- `epochs_iters`：Epochs 或者 Steps，模型对训练数据的重复学习次数，一般来说，轮次越大，模型训练时间越长，模型精度越高，但是如果设置特别大，可能会导致模型过拟合。如果对轮次没有特别的要求，可以使用默认值进行训练。
- `batch_size`：批大小，由于训练数据量一般较大，模型每轮次的训练是分批读取数据的，批大小是每一批数据的数据量，和显存直接相关，批大小越大模型训练的速度越快，显存占用越高。
- `class_num`：类别数量，数据集中的类别数（若有类别概念），由于类别数量和数据集直接相关，我们无法填充默认值，请根据数据校验的结果进行填写，类别数量需要准确，否则可能引起训练失败。
- `learning_rate`：学习率，模型训练过程中梯度调整的步长，通常与批大小成正比例关系，学习率设置过大可能会导致模型训练不收敛，设置过小可能会导致模型收敛速度过慢。在不同的数据集上学习率可能不同，对结果影响较大，需要不断调试。

对于时序任务模块，基础参数包括`time_col`，`target_cols`，`group_id`，`static_cov_cols`，`freq`，`seq_len`。相关的参数解释如下：
- `time_col`：时间列，须结合自己的数据设置时间序列数据集的时间列的列名称。
- `target_cols`：目标变量列，须结合自己的数据设置时间序列数据集的目标变量的列名称，可以为多个，多个之间用','分隔。
- `group_id`：分组列名，时序数据中表示分组标识的列名称。
-  `static_cov_cols`：标签变量，代表时序的类别，须结合自己的数据设置类别的列名称，如：label。
- `freq`：频率，须结合自己的数据设置时间频率，如：1min、5min、1h。
- `seq_len`：群组编号，须结合自己的数据设置指定群组编号的列名称, 如：group_id, 群组编号表示的是每个时序样本。

## 2. 进阶参数
- `resune_path`：断点训练权重：在模型训练过程中发生人为或意外终止的情况时，加载训练中断之前保存的断点权重路径，完成继续训练，避免算力资源浪费。
- `pretrain_weight_path`：预训练权重：基于已经在大数据集上训练好的模型权重进行微调训练，可提高模型训练开始前的初始经验，提高训练效率。
- `warmup_steps`：热启动步数(WarmUp Steps)：在训练初始阶段以较小学习率缓慢增加到设置学习率的批次数量，该值的设置可以避免模型在初始阶段以较大学习率迭代模型最终破坏预训练权重，一定程度上提升模型的精度。
- `log_inteval`：log 打印间隔，训练日志中打印日志信息的批次数量间隔。
- `eval_inteval`：评估间隔，训练过程中对验证集进行评估的轮数间隔。
- `save_inteval`：保存权重间隔，训练过程中保存权重的轮数间隔。

**注：** 在**目标检测**和**实例分割**任务模块中，评估、保存间隔为统一参数 `eval_interval`。

## 3. 更多参数
在 PaddleX 中，除了常见的参数修改之外，也支持您修改更多的参数。
<!-- 这里简单解释不同config的逻辑，以及参数覆盖逻辑，廷权 -->
