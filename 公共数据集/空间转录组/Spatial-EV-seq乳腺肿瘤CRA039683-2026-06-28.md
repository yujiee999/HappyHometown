# Spatial-EV-seq乳腺肿瘤CRA039683

## 数据集名称

Spatial-EV-seq: a Platform for Spatial-resolved Tissue Extracellular Vesicle Biology

## 疾病/转移部位

- 主要相关部分：小鼠乳腺肿瘤组织，包含抗 PD-1 处理，用于分析 PD-L1+ 胞外囊泡（EV）空间分布与局部 CD8+ T 细胞功能障碍。
- 数据不是远处转移灶队列，没有脑、肺、骨或卵巢转移部位标签。
- 适合作为 EV—空间免疫分析的方法学和模型验证资源，不可作为人源器官嗜性结论的直接证据。

## 样本类型和样本量

- GSA accession：`CRA039683`
- BioProject：`PRJCA058161`
- 物种：Mus musculus
- 共 14 个实验、14 个 BioSample、14 个 runs、28 个双端 FASTQ 文件，约 493.65 GB。
- 乳腺组织相关样本 12 个：`ZLQ`、`ZLH`、`sample1`–`sample9`、`Breast-PDX`。
- 肝组织方法验证样本 2 个：`HepG2-1`、`HepG2-2`。
- GSA BioSample 标题将 `ZLH` 与 `sample1`–`sample9` 标为 anti-PD-1 treatment，将 `ZLQ`、`Breast-PDX` 和两个 HepG2 样本标为 none treatment；但 `sample1`–`sample9` 的精确组别、时间点和重复结构不能仅凭 GSA 元数据确定，必须回查补充材料。

## 平台

- 测序平台：Illumina NovaSeq 6000
- 空间层：组织 EV 捕获、适配体-RCA 荧光成像、邻切片空间转录组及图像配准
- 计算层：Python/R/shell；三点仿射配准、荧光相关、stLearn CCI、基因-EV 共定位与区域差异分析

## 可获得的数据层级

- 原始测序：双端 FASTQ，GSA 公开下载。
- 空间转录组：论文代码按 10x Visium 风格 `filtered_feature_bc_matrix.h5` 与 `spatial/` 目录设计；GSA 原始 reads 需要按论文流程重建空间表达对象。
- EV/蛋白空间信号：论文 Source Data、Supplementary Data 和显微图像；是否包含全部原始高分辨率图像及可直接复用坐标表，需要下载后核对。
- 代码：公开 GitHub 仓库，含配准、信号匹配、CCI、区域差异和可视化脚本。

## 下载入口或 accession

- GSA：https://ngdc.cncb.ac.cn/gsa/browse/CRA039683
- BioProject：https://ngdc.cncb.ac.cn/bioproject/browse/PRJCA058161
- HTTPS 原始数据：https://download.cncb.ac.cn/gsa6/CRA039683
- 论文：https://doi.org/10.1038/s41587-026-03192-3
- PubMed：https://pubmed.ncbi.nlm.nih.gov/42362724/
- 代码：https://github.com/BuckyEv/Spatial-EV-seq

## 关联论文

Wen Q, Na X, Lu Y, et al. Spatially resolved profiling of extracellular vesicles in tissues with Spatial-EV-seq. Nature Biotechnology. 2026. DOI: `10.1038/s41587-026-03192-3`。

## 可用于验证的具体假设

- PD-L1+ EV 富集区是否伴随 CD8+ T 细胞耗竭、细胞毒性下降和抗 PD-1 低反应状态。
- EV-high 与 EV-low 区域的髓系细胞比例、TAM 状态和免疫检查点配体-受体网络是否不同。
- EV 空间富集是否比单纯 `Cd274/PD-L1` 转录本表达更能界定局部免疫抑制生态位。
- 将方法迁移到用户脑转移样本后，可检验 CD163+ 巨噬细胞邻域是否与 PD-L1+ EV 或其他髓系 EV 共定位；这是迁移性假设，不是本数据集直接覆盖的生物学问题。
- 可用作者的区域化分析框架比较 PT 与 MT 中 EV/蛋白信号邻域，但当前数据本身不含配对 PT/MT。

## 下载难度和注意事项

- 下载难度：高。原始数据约 493.65 GB，建议先下载元数据、论文 Source Data 和代码，确认目标样本后再按 run 获取 FASTQ。
- 关键限制：不是人源数据、不是转移灶、不是单细胞转录组；主要价值是方法复现和空间免疫机制验证。
- GSA 中所有样本的 organism 均登记为小鼠，`HepG2` 和 `Breast-PDX` 命名可能反映移植来源而非测序物种；跨物种 reads 处理时需检查是否采用联合参考基因组。
- `sample1`–`sample9` 的命名缺乏生物学语义。下载前必须根据补充表建立 run—处理—重复—切片对应表，不能只按文件名合并。
- 相邻切片空间配准需要人工地标和图像质控；区域边界、EV 阈值和配准误差都应做敏感性分析。
- 代码仓库虽然公开，但部分脚本依赖 DynamicISS、stLearn、Scanpy、Seurat、DESeq2 和 GUI 库，环境复现成本中等偏高。

## 与现有笔记的连接

- [[Spatial-EV-seq空间胞外囊泡-2026]]
- [[外泌体整合素器官嗜性转移-2015]]
- [[TNBC癌症干细胞外泌体免疫逃逸-2026]]
- [[脑转移空间免疫CD163巨噬细胞-2026]]
