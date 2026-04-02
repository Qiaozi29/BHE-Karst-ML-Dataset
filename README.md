# 🌍 岩溶地下水对地埋管换热器换热性能影响的研究的机器学习预测数据集
**Dataset for ML-based Borehole Heat Exchanger (BHE) Prediction in Karst Geology**

> **🏆 贵州大学硕士学位论文支撑数据与代码**
> 本项目仓库包含了《岩溶地下水对地埋管换热器换热性能影响的研究》一文中所使用的完整数值模拟数据集与 Python 机器学习源代码。

---

## 📌 项目简介 (Project Overview)

针对岩溶地区复杂的水文地质条件（涵盖无地下水、孔隙渗流、裂隙流、地下暗河管道流等多种介质形态），本研究通过**拉丁超立方抽样（LHS）**结合 **COMSOL Multiphysics** 瞬态传热模拟，结合第四章的瞬态数据，构建了包含 **46,110 组**独立工况的大规模地埋管换热器（BHE）运行数据集。

配套的 Python 代码展示了如何利用 `Optuna` 框架对 `XGBoost`、`LightGBM` 等集成学习算法进行自动化超参数寻优，并通过 `SHAP` 框架对预测结果进行物理机制的深度可解释性分析。

---

## 📊 数据集字典 (Data Dictionary)

数据文件为仓库根目录下的 **`JQXX.csv`**，总计 **46,110 行**（不含表头）。数据共包含 21 列，详细物理含义与单位如下：

### 1. 🎯 标识与目标变量 (Identifiers & Target)
| 列名 (Column) | 参数名称 (Description) | 单位 (Unit) | 备注说明 (Note) |
| :--- | :--- | :---: | :--- |
| `Run_ID` | 实验组编号 | - | 用于区分不同 LHS 抽样工况，**不参与机器学习训练**。 |
| `Time` | 运行时间 | d | 瞬态换热运行时间。 |
| **`HCLM`** | **延米换热量** | **W/m** | **🎯 机器学习的最终预测目标 (Target)**。 |

### 2. 🌐 模型公共参数 (Public Parameters)
| 列名 (Column) | 参数名称 (Description) | 单位 (Unit) |
| :--- | :--- | :---: |
| `sh` | 地埋管埋深 | m |
| `dr` | 岩土导热率 | W/(m·K) |
| `rr` | 岩土恒压热容 | J/(kg·K) |
| `T_chu` | 岩土初始温度 | ℃ |
| `kb` | 埋管回填材料导热系数 | W/(m·K) |
| `v` | 埋管进口流速 | m/s |
| `T_IN` | 埋管进口温度 | ℃ |

### 3. 💧 水文地质与介质特征 (Hydrogeological Parameters)
| 列名 (Column) | 参数名称 (Description) | 单位 (Unit) | 备注说明 (Note) |
| :--- | :--- | :---: | :--- |
| `Medium_Type`| 介质类型 | Category | 0=无水, 1=孔隙, 2=裂隙, 3=管道 *(需独热编码)* |
| `Tf` | 地下水的温度 | ℃ | 仅在存在地下水时有效 |
| `WZ` | 含水介质埋深 | m | 定位含水层/裂隙/管道距地表的距离 |
| `LSX` | X 向地下水流速 | m/s | 渗流/裂隙流/管道流的水平速度分量 |
| `LSY` | Y 向地下水流速 | m/s | 渗流/裂隙流/管道流的水平速度分量 |

### 4. 🪨 特定介质专属参数 (Specific Medium Parameters)
*(注：当 `Medium_Type` 不对应此介质时，以下相关参数值为 0)*
| 列名 (Column) | 参数名称 (Description) | 单位 (Unit) | 所属介质类型 |
| :--- | :--- | :---: | :---: |
| `SLHD` | 渗流厚度 | m | 🟩 孔隙介质 (Type=1) |
| `kx` | 孔隙率 | 1 | 🟩 孔隙介质 (Type=1) |
| `df` | 裂隙孔径 | mm | 🟦 裂隙介质 (Type=2) |
| `lxts` | 裂隙条数 | 1 | 🟦 裂隙介质 (Type=2) |
| `gdk` | 管道孔径 | mm | 🟪 管道介质 (Type=3) |
| `gdts` | 管道条数 | 1 | 🟪 管道介质 (Type=3) |

---

## 🚀 代码使用说明 (Usage)

本仓库提供的主要 Python 脚本集成了从数据清洗、模型调优到可解释性绘图的全套学术分析流程。

### 🛠️ 核心依赖库 (Requirements)
建议使用 Python 3.9+ 环境运行，需安装以下依赖：
```bash
pip install pandas numpy scikit-learn xgboost lightgbm optuna shap matplotlib
📜 学术声明 (Declaration)
本数据集与代码仅供学术研究与同行交流使用。如需在您的研究中复用本数据集或代码逻辑，请在您的学术论文或报告中引用本人的硕士学位论文。
Data and code are provided "as is" for academic replication, facilitating further research in geothermal energy and machine learning.
