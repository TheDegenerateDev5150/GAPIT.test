# GAPIT Agent 使用手册

## 目录

1. [概述](#概述)
2. [系统要求](#系统要求)
3. [安装指南](#安装指南)
   - [3.1 安装TRAE平台](#31-安装trae平台)
   - [3.2 安装GAPIT Agent](#32-安装gapit-agent)
4. [快速开始](#快速开始)
5. [核心功能](#核心功能)
   - [5.1 GWAS分析支持](#51-gwas分析支持)
   - [5.2 基因组选择（GS）分析](#52-基因组选择gs分析)
   - [5.3 问题诊断和解决](#53-问题诊断和解决)
6. [详细使用指南](#详细使用指南)
   - [6.1 数据准备](#61-数据准备)
   - [6.2 参数设置](#62-参数设置)
   - [6.3 运行分析](#63-运行分析)
   - [6.4 结果解读](#64-结果解读)
7. [高级功能](#高级功能)
   - [7.1 多性状分析](#71-多性状分析)
   - [7.2 交叉验证](#72-交叉验证)
   - [7.3 代码修改管理](#73-代码修改管理)
8. [故障排除](#故障排除)
9. [常见问题解答](#常见问题解答)
10. [附录](#附录)

---

## 概述

GAPIT Agent是基于TRAE平台的遗传关联分析专家智能体，专门为GAPIT（Genome Association and Prediction Integrated Tool）软件用户提供专业支持。本智能体由GAPIT软件作者团队开发，集成了多年的用户支持经验和专业知识。

### 主要特点

- **专业指导**：提供GAPIT软件的专业使用指导
- **智能诊断**：能够诊断和解决各种GAPIT使用问题
- **代码生成**：根据用户需求生成标准的GAPIT分析代码
- **结果解读**：帮助用户理解和解读GAPIT分析结果
- **多方法支持**：支持GLM、MLM、SUPER、MLMM、FarmCPU、BLINK等多种GWAS方法

## 系统要求

### 硬件要求
- **操作系统**：Windows 10/11, macOS 10.15+, Ubuntu 18.04+
- **内存**：至少8GB RAM（推荐16GB以上）
- **存储空间**：至少2GB可用空间
- **处理器**：64位多核处理器

### 软件要求
- **TRAE平台**：最新版本
- **R语言**：R 4.0.0或更高版本
- **GAPIT包**：GAPIT 3.0或更高版本
- **必要R包**：ggplot2, reshape2, dplyr, tidyr等

## 安装指南

### 3.1 安装TRAE平台

#### Windows系统
1. 访问TRAE官方网站下载安装程序
2. 运行安装程序，按照向导完成安装
3. 启动TRAE平台，创建账户并登录

#### macOS系统
1. 访问TRAE官方网站下载DMG文件
2. 双击DMG文件，将TRAE拖拽到Applications文件夹
3. 在Applications中启动TRAE，创建账户并登录

#### Linux系统
1. 访问TRAE官方网站下载AppImage文件
2. 给文件添加执行权限：`chmod +x TRAE-*.AppImage`
3. 运行：`./TRAE-*.AppImage`

### 3.2 安装GAPIT Agent

#### 步骤1：下载技能包
将`gapit-expert-skill-with-functions-updated.zip`文件保存到本地目录。

#### 步骤2：导入智能体
1. 打开TRAE平台
2. 点击"智能体管理" → "导入智能体"
3. 选择下载的ZIP文件
4. 等待导入完成

#### 步骤3：验证安装
1. 在TRAE聊天界面输入：`@gapit-expert 你好`
2. 智能体应回复欢迎信息
3. 测试基本功能：`@gapit-expert 如何安装GAPIT包`

## 快速开始

### 第一次使用GAPIT Agent

```r
# 1. 启动智能体
在TRAE聊天界面输入：@gapit-expert

# 2. 检查环境
问：请检查我的R环境是否适合运行GAPIT

# 3. 准备数据
问：我的表型数据格式应该是怎样的？

# 4. 运行简单测试
问：请帮我生成一个简单的GWAS测试代码
```

### 文件夹结构设置

建议按以下结构组织您的项目：

```
项目文件夹/
├── data/
│   ├── genotype/
│   │   ├── mdp_genotype_test.hmp.txt    # 基因型数据
│   │   └── mdp_numeric.txt              # 数值型基因型
│   ├── phenotype/
│   │   └── mdp_traits.txt               # 表型数据
│   └── snp_info/
│       └── mdp_SNP_information.txt      # SNP信息
├── scripts/
│   ├── gwas_analysis.R                  # GWAS分析脚本
│   └── gs_analysis.R                    # GS分析脚本
└── results/
    ├── gwas_results/                    # GWAS结果
    └── gs_results/                      # GS结果
```

## 核心功能

### 5.1 GWAS分析支持

#### 支持的模型
1. **GLM（广义线性模型）**：快速初步筛选
2. **MLM（混合线性模型）**：校正群体结构
3. **SUPER**：迭代优化，提高检测功效
4. **MLMM**：多基因位点同时考虑
5. **FarmCPU**：固定和随机效应分离
6. **BLINK**：贝叶斯框架，最高检测功效

#### 典型对话示例
```
用户：我想用BLINK模型进行GWAS分析
智能体：好的，请提供您的表型数据和基因型数据路径
用户：表型数据：data/phenotype/mdp_traits.txt
      基因型数据：data/genotype/mdp_numeric.txt
智能体：正在为您生成BLINK分析代码...
```

### 5.2 基因组选择（GS）分析

#### 支持的方法
1. **GBLUP**：基于基因组关系的混合模型
2. **GAGBLUP**：GWAS辅助的基因组选择
3. **MAS**：标记辅助选择

#### GAGBLUP优势
- **少QTN条件**：大幅领先于GBLUP
- **适中QTN**：表现最优
- **多QTN条件**：下降较慢，较早到达平稳

### 5.3 问题诊断和解决

#### 常见问题类型
1. **安装问题**：GAPIT包安装失败
2. **内存问题**：大数据集内存不足
3. **收敛问题**：模型无法收敛
4. **格式问题**：数据格式不正确

## 详细使用指南

### 6.1 数据准备

#### 表型数据格式
```r
# 标准格式：CSV或TXT文件
# 第一列必须是样本ID（Taxa）
# 后续列为性状数据

示例：
Taxa,Height,Weight,Yield
Sample1,150.2,45.3,25.6
Sample2,148.7,43.8,24.9
Sample3,152.1,46.7,26.3
```

#### 基因型数据格式
1. **HapMap格式**：`.hmp.txt`文件
2. **数值型格式**：`.txt`文件，0/1/2编码
3. **PLINK格式**：`.bed/.bim/.fam`文件

### 6.2 参数设置

#### 关键参数说明

| 参数 | 类型 | 默认值 | 作用 |
|------|------|--------|------|
| `model` | 字符向量 | "MLM" | 关联分析模型 |
| `PCA.total` | 整数 | 3 | PCA主成分数量 |
| `file.output` | 逻辑值 | TRUE | 是否输出结果文件 |
| `buspred` | 逻辑值 | FALSE | 是否进行基因组选择预测 |
| `lmpred` | 逻辑向量 | c(FALSE) | FALSE=GAGBLUP, TRUE=MAS |

#### 模型选择决策树
```
开始
├── 样本量 < 100? → GLM
├── 有明显群体结构？ → MLM（PCA.total=5）
├── 数据量很大，计算资源有限？ → CMLM
├── 需要精细定位？ → SUPER
├── 多基因控制性状？ → MLMM
├── 计算效率要求高？ → FarmCPU
└── 默认 → BLINK（推荐）
```

### 6.3 运行分析

#### 基本GWAS分析代码
```r
library(GAPIT)

# 1. 读取数据
myY <- read.table("data/phenotype/mdp_traits.txt", head = TRUE)
myGD <- read.table("data/genotype/mdp_numeric.txt", head = TRUE)
myGM <- read.table("data/snp_info/mdp_SNP_information.txt", head = TRUE)

# 2. 运行GAPIT
myGAPIT <- GAPIT(
  Y = myY[, c(1, 2)],  # 分析第一个性状
  GD = myGD,
  GM = myGM,
  PCA.total = 3,
  model = c("BLINK"),
  file.output = TRUE,
  file.path = "results/gwas_results/"
)

# 3. 保存结果
saveRDS(myGAPIT, file = "results/gwas_results/gapit_results.rds")
```

#### 基因组选择分析代码
```r
# GAGBLUP分析示例
myGAPIT_GS <- GAPIT(
  Y = myY[, c(1, 2)],  # 参考群体表型
  GD = myGD,           # 全部基因型数据
  GM = myGM,
  PCA.total = 3,
  model = c("BLINK"),
  buspred = TRUE,      # 启用预测
  lmpred = c(FALSE),   # 使用GAGBLUP
  file.output = TRUE,
  file.path = "results/gs_results/"
)
```

### 6.4 结果解读

#### 主要输出文件

| 文件类型 | 文件名模式 | 内容说明 |
|----------|------------|----------|
| GWAS结果 | `GAPIT.Association.GWAS_Results.*.csv` | 主要关联分析结果 |
| 曼哈顿图 | `GAPIT.Association.Manhattan_*.pdf` | 全基因组和染色体级曼哈顿图 |
| QQ图 | `GAPIT.Association.QQ.*.pdf` | Q-Q图，检查λ值 |
| PCA图 | `GAPIT.Genotype.PCA_*.pdf` | 主成分分析结果 |
| 预测结果 | `GAPIT.Association.Prediction_results.*.csv` | 基因组选择预测结果 |

#### 结果解读要点

1. **λ值**：理想情况λ ≈ 1.0
   - λ > 1.05：群体结构未充分校正
   - λ < 0.95：可能过度校正

2. **显著阈值**：
   - Bonferroni校正：`p < 0.05/n`（n为标记数）
   - 建议阈值：`p < 1e-5`作为初步筛选标准

3. **区域聚集**：检查是否有多个显著SNP聚集，可能表示真实信号区域

## 高级功能

### 7.1 多性状分析

#### 避免文件覆盖的策略
```r
# 多性状分析最佳实践
trait_names <- colnames(myY)[-1]  # 排除Taxa列

for(trait in trait_names) {
  # 为每个性状创建独立输出目录
  output_dir <- paste0("results/gwas_results/", trait)
  dir.create(output_dir, showWarnings = FALSE)
  
  # 运行分析
  myGAPIT <- GAPIT(
    Y = myY[, c("Taxa", trait)],
    GD = myGD,
    GM = myGM,
    PCA.total = 3,
    model = c("BLINK"),
    file.output = TRUE,
    file.path = output_dir
  )
}
```

### 7.2 交叉验证

#### 标准交叉验证设置
```r
# 20次重复5折交叉验证
library(caret)

n_repeats <- 20
cv_results <- data.frame()

for(rep in 1:n_repeats) {
  set.seed(198521 + rep)
  
  # 创建5折
  folds <- createFolds(1:nrow(myY), k = 5, returnTrain = TRUE)
  
  for(fold in 1:5) {
    # 划分训练集和验证集
    train_indices <- folds[[fold]]
    valid_indices <- setdiff(1:nrow(myY), train_indices)
    
    # 运行分析...
  }
}
```

### 7.3 代码修改管理

#### 智能体代码修改处理流程
```
开始
├── 用户提出可能涉及代码修改的需求
├── 智能体明确提示：是否改变原始GAPIT代码？
│   ├── 用户确认修改 → 继续
│   └── 用户拒绝 → 提供替代方案
├── 评估修改合理性和必要性
├── 创建原始代码备份
├── 实施代码修改
├── 验证修改效果
├── 记录修改详情
├── 上报创造者确认
│   ├── 大多数人都需求 → 更新整体GAPIT代码
│   └── 少数人需求 → 隐藏修改
└── 返回处理结果
```

## 故障排除

### 常见错误及解决方案

#### 错误1：内存不足
```
错误信息：cannot allocate vector of size...
解决方案：
1. 增加R内存限制：memory.limit(size=16000)
2. 使用内存优化模型：CMLM或FarmCPU
3. 分批处理数据
```

#### 错误2：收敛失败
```
错误信息：convergence failed
解决方案：
1. 增加迭代次数
2. 调整模型参数
3. 简化模型
```

#### 错误3：文件不存在
```
错误信息：object 'myY' not found
解决方案：
1. 检查文件路径是否正确
2. 确认文件是否存在
3. 检查文件读取代码
```

## 常见问题解答

### Q1: GAPIT Agent和GAPIT软件有什么区别？
**A:** GAPIT Agent是运行在TRAE平台上的智能助手，专门为GAPIT软件用户提供使用指导、问题诊断、代码生成等服务。GAPIT软件是实际的统计分析工具。

### Q2: 如何选择合适的GWAS模型？
**A:** 根据数据特点选择：
- 小样本：GLM
- 有群体结构：MLM
- 需要高检测功效：BLINK或FarmCPU
- 多基因性状：MLMM

### Q3: GAGBLUP在什么条件下表现最好？
**A:** GAGBLUP在QTN数目适中时（N=10-15）表现最优。在少QTN条件下与MAS相当，在多QTN条件下优于MAS但计算较慢。

### Q4: 如何评估GWAS结果的可靠性？
**A:** 检查以下指标：
1. λ值接近1.0
2. QQ图基本在直线上
3. 显著位点有生物学意义
4. 多个方法检测到相同位点

### Q5: 多性状分析时如何避免文件覆盖？
**A:** 为每个性状创建独立的输出目录，或使用`file.path`参数指定不同路径。

## 附录

### A. 文件命名规则

#### 基础命名结构
```
GAPIT.[模块].[文件类型].[模型].[性状]([版本]).扩展名
```

#### 版本标识含义
- **(Kansas)**：逐步回归模型结果 - 零星单个显著位点
- **(NYC)**：传统GWAS结果 - 连续显著信号区域

### B. 环境锁定标准流程

```r
# 环境锁定检查
check_environment <- function() {
  # 1. 检测操作系统
  os_type <- .Platform$OS.type
  
  # 2. 检测R环境
  r_version <- R.version$version.string
  
  # 3. 检测GAPIT安装状态
  gapit_installed <- "GAPIT" %in% installed.packages()[, "Package"]
  
  # 4. 返回检查结果
  return(list(
    os = os_type,
    r_version = r_version,
    gapit_installed = gapit_installed
  ))
}
```

### C. 反馈模板

当您遇到问题或需要帮助时，请提供以下信息：

```
[问题描述]
- 具体错误信息：
- 相关代码：
- 期望结果：
- 实际结果：

[环境信息]
- 操作系统：
- R版本：
- GAPIT版本：
- 数据规模：

[已尝试的解决方案]
1. 
2. 
3. 
```

### D. 联系支持

- **GAPIT交流群**：通过微信群获取实时支持
- **GitHub Issues**：报告技术问题和建议
- **Email支持**：联系GAPIT开发团队

---

**最后更新**：2026-06-30  
**版本**：GAPIT Agent v5.0  
**维护者**：GAPIT开发团队  

*注意：本手册会持续更新，建议定期检查新版本*
