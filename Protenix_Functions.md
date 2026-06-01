## Lamarck &nbsp; &nbsp; &nbsp; 2026-06-01
#### 该文档用于记录 server 上跑 Protenix 的各种命令
---

*236 机子的环境*
```bash
conda activate lmk_Protenix
```

*输入输出路径*
```bash
权重缓存:   /data/lmk/protenix_downloads   # 模型权重 + CCD 等依赖
输入目录:   /data/lmk/protenix_inputs      # 输入 JSON，运行后生成 MSA 缓存）
输出目录:   /data/lmk/protenix_outputs     # 预测结构 cif + 置信度 json
```

*GPU 选择*
```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2
```

---

> **01 蛋白质结构预测 -- |单任务|远程 MSA|默认参数|**
```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2 \
protenix pred \
  -i /data/lmk/protenix_inputs/ubiquitin.json \
  -o /data/lmk/protenix_outputs/ubiquitin
```

##### [Protenix 官方仓库](https://github.com/bytedance/Protenix)
