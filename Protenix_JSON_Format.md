## Lamarck &nbsp; &nbsp; &nbsp; 2026-06-01
#### 该文档用于展示 Protenix 输入 JSON 的常见格式（以蛋白质为例）
---

> **00 Protenix JSON 基本结构**
```json
[                                 
  {
    "name": "Example",             // 任务名，输出子目录以此命名
    "sequences": [                 // 该复合物包含的所有实体
      {
        "proteinChain": {          // 实体类型：proteinChain / dnaSequence / rnaSequence / ligand / ion
          "sequence": "...",       // 氨基酸序列
          "count": 1               // 拷贝数，同源 N 聚体写 N
        }
      }
    ]
  }
]
```
> seed 由命令行 `-s/--seeds` 指定，不写在 JSON 里（除非 `--use_seeds_in_json`）；无 dialect / version 字段

---

> **01 单链蛋白**
```json
[
  {
    "name": "ubiquitin",
    "sequences": [
      {
        "proteinChain": {
          "sequence": "MQIFVKTLTGKTITLEVEPSDTIENVKAKIQDKEGIPPDQQRLIFAGKQLEDGRTLSDYNIQKESTLHLVLRLRGG",
          "count": 1
        }
      }
    ]
  }
]
```

---

> **02 同源多聚体**
```json
[
  {
    "name": "homodimer",
    "sequences": [
      {
        "proteinChain": {
          "sequence": "MQIFVKTLTGKTITLEVEPSDTIENVKAKIQDKEGIPPDQQRLIFAGKQLEDGRTLSDYNIQKESTLHLVLRLRGG",
          "count": 2
        }
      }
    ]
  }
]
```

---

> **03 异源多聚体**
```json
[
  {
    "name": "heterodimer",
    "sequences": [
      {
        "proteinChain": {
          "sequence": "MKTAYIAKQRQISFVKSHFSRQLEERLGLIEVQAPILSRVGDGTQDNLSGAEKAVQ",
          "count": 1
        }
      },
      {
        "proteinChain": {
          "sequence": "GMRESYANENQFGFKTINSDIHKIVIVGGYGKLGGLFARYLRASGYPISILDRED",
          "count": 1
        }
      }
    ]
  }
]
```

---

##### [Protenix 官方仓库](https://github.com/bytedance/Protenix)
