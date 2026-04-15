===========================================================
                    ncNet 模型复现任务报告 (2026-04-16)
项目背景：复现 IEEE VIS 2021 论文 ncNet (Natural Language to Visualization)
实验环境：智星云 Tesla V100 节点
最终状态：成功 (SUCCESS) - 准确率对齐论文 SOTA 水平
--------------------------------------------------------------------------------

1. 硬件与驱动审计 (Hardware & Environment)
------------------------------------------
* 核心显卡：NVIDIA Tesla V100 (32GB 显存 / Volta 架构)
* 驱动版本：550.127.05 / CUDA 12.4
* 虚拟环境：conda activate ncnet (Python 3.8 / PyTorch 1.7.1)

2. 数据预处理链路 (Data Pipeline)
---------------------------------
    - 运行 ./preprocessing/process_dataset.py 完成了特征工程，将自然语言、图表模板
      和数据库结构拼接为模型可识别的序列。


3. 训练协议与监控 (Training Protocol)
-------------------------------------
* 训练规模：
    - 迭代轮次 (Epochs)：50 (依据作者官方默认值与收敛趋势)
    - 批大小 (Batch Size)：128 
* 性能监控：
    - 显卡利用率：训练期间 GPU-Util 稳定在 17% - 45%。
    - 显存占用：模型与数据载入后峰值占用约为 4328MiB。
    - 监控日志：./logs/gpu_training_monitor.csv

4. 核心指标审计 (Final Benchmark Results)

旧模型测试结果
<img width="1722" height="871" alt="e780e7e76ff5b6a85271c7d7424a9ffb" src="https://github.com/user-attachments/assets/e7fa5b6f-3942-4994-b28e-2856c0323c2e" />
训练后模型结果
<img width="580" height="275" alt="image" src="https://github.com/user-attachments/assets/2ae359cd-7d02-4bfb-9dcc-863e593050cc" />
-----------------------------------------
对比项                | 旧模型 (原始留存) | 新模型 | 提升幅度
----------------------|-------------------|---------------------|---------
ncNet w/o template    | 49.31%            | 64.43%              | +15.12%
ncNet with template   | 58.13%            | 74.23%              | +16.10%
Overall Accuracy      | 53.72%            | 69.33%              | +15.61%

* 结论分析：新模型准确率突破 69%，已完美复现原论文在 nvBench 上的基准性能。

5. 交付产物清单 (Handover Artifacts)
------------------------------------
* 最优模型权重：./save_models/model_best.pt 
* 训练全量日志：./logs/ncnet_training_20260416.log
* 资源开销记录：./logs/gpu_training_monitor.csv（显卡状态监控）

================================================================================
                                 REPORT END
================================================================================
