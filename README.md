# 基于弗明汉心脏研究队列的临床数据清洗与基线特征分析
# (Clinical Data Cleaning and Baseline Characteristics Analysis)

本项目基于著名的临床试验数据集——**弗明汉心脏研究 (Framingham Heart Study)** 随访队列（5,209例受试者），模拟了一个完整的临床数据统计分析与数据合规清洗（Clinical Data Cleaning）工作流。

---

## 🛠️ 技术栈 (Tech Stack)
* **核心工具：** SAS Studio (SAS OnDemand for Academics)
* **SAS 核心技术：** DATA Step, PROC FORMAT, PROC MEANS, PROC FREQ (卡方检验), PROC SGPLOT (数据可视化), 数字化留痕 (Audit Trail)

---

## 📐 项目核心描述 (基于 STAR 法则)

### 📌 情境 (Situation)
在复现心血管疾病真实世界研究（RWS）的课题中，面对包含 **5,209 条** 原始受试者临床随访数据的复杂库（`SASHELP.HEART`）。原始数据存在手工录入导致的**逻辑冲突**（如基线年龄大于死亡年龄、年龄为负数）、**关键临床指标缺失**（如胆固醇数据丢失）以及**连续变量未分箱**等问题，无法直接用于后续的统计学建模。

### 📌 任务 (Task)
必须在严格遵守临床试验数据质量规范的前提下，利用 SAS 快速搭建一条**可追溯的自动化清洗流**：
1. 清除所有逻辑错漏样本，确保数据一致性；
2. 对缺失的关键生理指标进行合规填补；
3. 完成特征工程与变量离散化分箱（Discretization）；
4. 输出合规的标准化临床分析数据集（Analysis Dataset, ADS）以及受试者基线特征表（Demographics Base Table）。

### 📌 行动 (Action)
* **数据清洗与排查：** 编写 `DATA 步` 与 `IF-THEN` 过滤语句，自动化剔除 `AgeAtStart < 0` 及 `AgeAtStart > AgeAtDeath` 的逻辑冲突样本。
* **缺失值填补 (Imputation)：** 运用 `PROC MEANS` 锁定收缩压（Systolic）的行业分布特征，并采用中位数对缺失字段进行合规填补。
* **特征工程与标准编码：** 利用 `PROC FORMAT` 自定义临床分箱标准，将连续的胆固醇数值（Cholesterol）转化为临床标准的分类等级（正常/边缘升高/高胆固醇）；同时通过差值计算派生出新变量——脉压差（Pulse_Pressure）。
* **统计学分析与可视化：** 运用 `PROC FREQ` 配合**卡方检验 (Chi-Square Test)** 自动化评估胆固醇水平与患者最终生存结局的相关性；利用 `PROC SGPLOT` 绘制不同性别下脉压差的**箱线图 (Box Plot)**，高效识别并展示基线指标的离群分布。

### 📌 结果 (Result)
* **零报错交付：** 成功构建了包含 5,000+ 样本的标准化临床分析数据集（ADS）。
* **合规与数字化留痕：** 全流程实现 100% 自动化代码留痕，SAS 运行日志（Log）保持 **Zero Error / Zero Warning（无任何红字报错与隐式转换警告）**，确保了临床数据的可追溯性（Audit Trail）。
* **效率提升：** 相比传统人工 Excel 筛查，数据清洗与基线报表生成的效率提升了数倍，单人独立高效地完成了原本需要多人协作的临床统计前期工作。

---

## 📂 仓库文件指南 (Repository Structure)
* `/clinical_data`: 存放清洗前后的临床数据集说明（本研究直接调用 SASHELP 逻辑库）。
* `/code`: 存放核心 SAS 清洗与分析源代码。
* `/results`: 存放自动生成的基线特征统计表、卡方检验报告及脉压差分布箱线图。
