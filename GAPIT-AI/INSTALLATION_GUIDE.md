# GAPIT Agent 安装指南

## 快速安装步骤

### 步骤1：安装TRAE平台

#### Windows系统
1. 访问 [TRAE官方网站](https://trae.ai)
2. 下载Windows安装程序
3. 运行安装向导，按默认设置完成安装
4. 启动TRAE，创建账户并登录

#### macOS系统
1. 访问TRAE官方网站下载DMG文件
2. 双击DMG文件，将TRAE拖拽到Applications文件夹
3. 在Applications中启动TRAE
4. 如果提示"无法打开"，进入系统设置 → 安全性与隐私 → 允许运行

#### Linux系统
```bash
# 下载AppImage文件
wget https://trae.ai/downloads/TRAE-latest.AppImage

# 添加执行权限
chmod +x TRAE-latest.AppImage

# 运行TRAE
./TRAE-latest.AppImage
```

### 步骤2：下载GAPIT Agent技能包

在GAPIT5文件夹中找到以下文件：
- `gapit-expert-skill-with-functions-updated.zip` - 智能体技能包
- `GAPIT_Agent_Complete_Manual.md` - 完整使用手册
- `example_project/` - 示例项目

### 步骤3：导入智能体到TRAE

```mermaid
graph TD
    A[启动TRAE] --> B[点击"智能体管理"]
    B --> C[选择"导入智能体"]
    C --> D[浏览文件]
    D --> E[选择gapit-expert-skill-with-functions-updated.zip]
    E --> F[点击"导入"]
    F --> G[等待30-60秒]
    G --> H[导入成功]
```

详细步骤：
1. 打开TRAE平台
2. 点击左侧菜单的"智能体管理"
3. 点击"导入智能体"按钮
4. 浏览文件，选择下载的ZIP文件
5. 点击"导入"按钮
6. 等待导入过程完成

### 步骤4：验证安装

在TRAE聊天界面进行测试：

```
测试1：基本功能
输入：@gapit-expert 你好
预期：智能体回复欢迎信息

测试2：GWAS支持
输入：@gapit-expert 有哪些GWAS模型？
预期：列出GLM, MLM, SUPER, MLMM, FarmCPU, BLINK

测试3：代码生成
输入：@gapit-expert 生成一个简单的GLM分析代码
预期：生成完整的R代码
```

## 系统要求

### 硬件要求
- **操作系统**: Windows 10/11, macOS 10.15+, Ubuntu 18.04+
- **内存**: 至少8GB RAM（推荐16GB以上）
- **存储空间**: 至少2GB可用空间
- **处理器**: 64位多核处理器

### 软件要求
1. **TRAE平台**: 最新版本
2. **R语言**: R 4.0.0或更高版本
3. **RStudio**（可选）: RStudio 2023.09+

### R包依赖
```r
# 安装GAPIT包
install.packages("devtools")
devtools::install_github("jiabowang/GAPIT3")

# 安装必要依赖包
install.packages(c(
  "ggplot2",      # 绘图
  "reshape2",     # 数据重塑
  "dplyr",        # 数据处理
  "tidyr",        # 数据整理
  "data.table",   # 大数据处理
  "parallel",     # 并行计算
  "doParallel"    # 并行计算支持
))
```

## 详细安装说明

### 1. TRAE平台安装问题解决

#### 问题1：安装程序无法运行
- **解决方案**: 以管理员身份运行安装程序
- **Windows**: 右键点击安装程序 → "以管理员身份运行"
- **macOS**: 进入系统设置 → 安全性与隐私 → 允许运行

#### 问题2：网络连接问题
```r
# 设置代理（如有防火墙）
Sys.setenv(http_proxy="http://proxy.example.com:8080")
Sys.setenv(https_proxy="http://proxy.example.com:8080")

# 使用国内镜像
options(repos = c(CRAN = "https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
```

### 2. GAPIT包安装问题

#### 问题：GitHub安装失败
```r
# 方案1：手动下载安装
# 1. 访问 https://github.com/jiabowang/GAPIT3
# 2. 下载ZIP文件
# 3. 本地安装
devtools::install_local("path/to/GAPIT3-master.zip")

# 方案2：使用CRAN版本（如有）
install.packages("GAPIT")
```

#### 问题：依赖包安装失败
```r
# 逐个安装依赖包
required_packages <- c("ggplot2", "reshape2", "dplyr", "tidyr", 
                       "data.table", "parallel", "doParallel")

for (pkg in required_packages) {
  if (!require(pkg, character.only = TRUE)) {
    install.packages(pkg)
    library(pkg, character.only = TRUE)
  }
}
```

### 3. 智能体导入问题

#### 问题：导入失败或卡住
1. **检查文件完整性**: 确保ZIP文件没有损坏
2. **重新下载**: 从原始来源重新下载技能包
3. **清理缓存**: 重启TRAE平台
4. **检查权限**: 确保有写入权限

#### 问题：智能体不响应
1. **检查智能体状态**: 在"智能体管理"中查看状态
2. **重新激活**: 禁用后重新启用智能体
3. **更新TRAE**: 确保使用最新版本TRAE

## 配置指南

### 1. 项目文件夹设置

推荐的项目结构：
```
my_gapit_project/
├── data/
│   ├── genotype/          # 基因型数据
│   ├── phenotype/         # 表型数据
│   └── snp_info/          # SNP信息
├── scripts/               # R脚本
└── results/               # 分析结果
```

使用GAPIT Agent设置项目：
```
问：@gapit-expert 请帮我设置项目文件夹结构
智能体：正在为您生成文件夹创建脚本...
```

### 2. R环境配置

#### 设置工作目录
```r
# 在R中设置工作目录
setwd("C:/Users/YourName/Documents/GAPIT_Project")

# 或者在RStudio中使用Session → Set Working Directory
```

#### 检查环境
```
问：@gapit-expert 请检查我的R环境
智能体：正在检查R版本和包依赖...
```

### 3. 智能体权限设置

在TRAE智能体管理中配置：
1. **文件访问权限**: 允许读取/写入项目文件
2. **命令执行权限**: 允许执行R命令
3. **网络访问权限**: 允许下载R包

## 快速测试

### 测试1：基本功能测试
```r
# 使用GAPIT Agent生成测试代码
问：@gapit-expert 生成一个测试数据创建脚本

# 预期：生成创建模拟数据的R代码
```

### 测试2：GWAS分析测试
```r
# 使用示例数据测试
问：@gapit-expert 使用example_project数据运行GLM分析

# 预期：生成完整的GLM分析代码
```

### 测试3：结果解读测试
```
问：@gapit-expert 如何解读曼哈顿图？

智能体：曼哈顿图解读要点：
        1. 检查阈值线：绿色实线（Bonferroni），绿色虚线（FDR）
        2. 识别显著峰：高于阈值线的点
        3. 评估分布：均匀分布为正常
```

## 故障排除

### 常见问题及解决方案

| 问题 | 症状 | 解决方案 |
|------|------|----------|
| **TRAE启动失败** | 应用程序无法启动 | 重新安装，检查系统兼容性 |
| **智能体导入失败** | 导入过程卡住或报错 | 检查ZIP文件完整性，重启TRAE |
| **R包安装失败** | 安装过程中断或报错 | 使用代理，逐个安装依赖包 |
| **内存不足** | 分析过程中内存错误 | 清理内存，使用数值格式数据 |
| **文件路径错误** | 无法读取数据文件 | 使用绝对路径，避免中文路径 |

### 详细错误处理

#### 错误：`Error: Failed to install 'GAPIT' from GitHub`
```r
# 解决方案
# 1. 检查网络连接
# 2. 使用代理
Sys.setenv(http_proxy="http://your.proxy:port")
# 3. 手动下载安装
devtools::install_local("GAPIT3-master.zip")
```

#### 错误：`Error: cannot allocate vector of size XX MB`
```r
# 解决方案
# 1. 清理内存
rm(list = ls())
gc()
# 2. 使用数值格式
myGD <- read.table("mdp_numeric.txt", header = TRUE)
# 3. 分批处理
```

#### 错误：`Error in file(file, "rt") : cannot open the connection`
```r
# 解决方案
# 1. 检查文件是否存在
file.exists("data/mdp_traits.txt")
# 2. 使用绝对路径
setwd("C:/Users/Name/Documents/GAPIT_Project")
# 3. 避免特殊字符
```

## 更新和维护

### 1. 智能体更新

当有新版本时：
1. 下载新的技能包ZIP文件
2. 在TRAE中删除旧版本智能体
3. 导入新版本智能体
4. 验证功能

### 2. 数据备份

建议定期备份：
1. **原始数据**: 保持原始数据不变
2. **分析脚本**: 保存所有R脚本
3. **结果文件**: 备份重要结果
4. **环境快照**: 保存R环境信息

### 3. 性能优化

对于大规模数据：
1. 使用数值格式而非HapMap格式
2. 设置`PCA.total = 0`，手动计算PCA
3. 使用FarmCPU模型（计算效率高）
4. 分批处理大数据集

## 技术支持

### 获取帮助的途径

1. **智能体直接支持**:
   ```
   @gapit-expert help
   @gapit-expert 遇到问题
   @gapit-expert 技术支持
   ```

2. **文档资源**:
   - `GAPIT_Agent_Complete_Manual.md` - 完整手册
   - `Result_Interpretation_Guide.md` - 结果解读指南
   - `example_project/` - 示例项目

3. **社区支持**:
   - GAPIT用户微信群
   - GitHub Issues: https://github.com/jiabowang/GAPIT
   - 邮件支持: gapit.support@example.com

### 问题报告模板

报告问题时请提供：
1. **问题描述**: 详细说明遇到的问题
2. **复现步骤**: 如何重现问题
3. **环境信息**: 
   - TRAE版本
   - R版本
   - 操作系统
4. **错误信息**: 完整的错误信息和堆栈跟踪
5. **相关文件**: 如有，提供数据文件或代码

## 附录

### A. 安装检查清单

- [ ] TRAE平台安装完成
- [ ] TRAE账户创建并登录
- [ ] R语言安装（4.0.0+）
- [ ] GAPIT包安装成功
- [ ] 必要依赖包安装
- [ ] GAPIT Agent技能包下载
- [ ] 智能体导入TRAE成功
- [ ] 基本功能测试通过
- [ ] 示例项目运行成功

### B. 快速参考命令

| 命令 | 功能 | 示例 |
|------|------|------|
| `@gapit-expert help` | 显示帮助信息 | `@gapit-expert help` |
| `@gapit-expert models` | 显示GWAS模型 | `@gapit-expert models` |
| `@gapit-expert parameters` | 显示参数说明 | `@gapit-expert parameters` |
| `@gapit-expert check env` | 检查R环境 | `@gapit-expert check env` |
| `@gapit-expert setup project` | 设置项目 | `@gapit-expert setup project` |

### C. 联系信息

- **官方网站**: https://github.com/jiabowang/GAPIT
- **技术支持**: gapit.support@example.com
- **用户社区**: GAPIT用户微信群
- **文档更新**: 定期检查GitHub仓库

---

**最后更新**: 2026年7月6日  
**版本**: 1.0  
**适用智能体**: gapit-expert-skill-with-functions-updated.zip  

如需进一步帮助，请在TRAE平台中输入：`@gapit-expert help`