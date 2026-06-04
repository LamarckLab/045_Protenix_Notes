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
  -o /data/lmk/protenix_outputs
```

> **02 蛋白质结构预测 -- |批量|远程 MSA|默认参数|**

`-i` 指向目录，批量跑目录内所有 json
```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2 \
protenix pred \
  -i /data/lmk/protenix_inputs \
  -o /data/lmk/protenix_outputs
```

> **03 蛋白质结构预测 -- |批量|远程 MSA|多 seed|**

加 `-s 101,102,103` 指定多个 seed，每个 seed 独立采样并各自输出一个子目录
```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2 \
protenix pred \
  -i /data/lmk/protenix_inputs \
  -o /data/lmk/protenix_outputs \
  -s 101,102,103
```

> **04 蛋白质结构预测 -- |批量|远程 MSA|自定义 cycle/step/sample|**

`-c / -p / -e` 为自定义采样参数，调小可大幅提速，但牺牲质量

| 参数          | 默认值 | 含义                       |
| :------------ | :----: | :------------------------- |
| `-c` / cycle  |   10   | Pairformer 循环次数        |
| `-p` / step   |  200   | 扩散去噪步数               |
| `-e` / sample |   5    | 每个 seed 采样的候选结构数 |

```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2 \
protenix pred \
  -i /data/lmk/protenix_inputs \
  -o /data/lmk/protenix_outputs \
  -c 20 \
  -p 400 \
  -e 10
```

> **05 蛋白质结构预测 -- |批量|远程 MSA|选择模型|**

`-n` 切换模型；`--use_default_params true` 按所选模型自动套用 cycle/step（mini → 4/5，大模型 → 10/200）

| 模型名                          | 规模 | 备注                        |
| :------------------------------ | :--- | :-------------------------- |
| `protenix-v2`                   | 464M | 暂未开放下载（403）         |
| `protenix_base_default_v1.0.0`  | 368M | 默认，当前可使用的性能最强  |
| `protenix_base_20250630_v1.0.0` | 368M | 同档，训练集截止 2025-06-30 |
| `protenix_base_default_v0.5.0`  | 368M | 旧版 base                   |
| `protenix_mini_default_v0.5.0`  | mini | 快、精度低                  |
| `protenix_tiny_default_v0.5.0`  | tiny | 最快、精度最低              |

```bash
CUDA_DEVICE_ORDER=PCI_BUS_ID CUDA_VISIBLE_DEVICES=2 \
protenix pred \
  -i /data/lmk/protenix_inputs \
  -o /data/lmk/protenix_outputs \
  -n protenix_mini_default_v0.5.0 \
  --use_default_params true
```

##### [Protenix 官方仓库](https://github.com/bytedance/Protenix)
