# Comprehensive single-cell metabolic profiling identifies targets to sensitize triple-negative breast cancer to chemo-immunotherapy 精读笔记

## 文献信息

- 题目：Comprehensive single-cell metabolic profiling identifies targets to sensitize triple-negative breast cancer to chemo-immunotherapy
- 期刊：Cell Reports Medicine
- 年份：2026
- DOI：https://doi.org/10.1016/j.xcrm.2026.102659
- 作者：Ying Xu, Xin-Yi Liu, Hang Zhang, Li Chen, Han Wang, Zhi-Ming Shao, Yi Xiao, Yi-Zhou Jiang
- 研究对象：早期三阴性乳腺癌（TNBC）新辅助化疗或化疗联合免疫治疗队列
- 核心关键词：TNBC, scRNA-seq, single-cell metabolism, Compass, metabolic flux, lactate, MCT1, MCT4, GLUT3, myeloid cells, sacituzumab govitecan, anti-PD-1
- 原文 PDF：`E:/乳腺癌转移/文章/13-吕崇文-Comprehensive single-cell metabolic profiling identifies targets to sensitize triple-negative breast cancer to chemo-immunotherapy(1).pdf`

## 一句话总结

这篇文章用单细胞转录组结合代谢基因、代谢通路和 Compass 代谢通量推断，建立了 TNBC 肿瘤微环境的单细胞代谢图谱，并发现“髓系细胞通过 GLUT3 摄取葡萄糖、通过 MCT4 输出乳酸，肿瘤细胞通过 MCT1 回收乳酸并用于生长”的代谢互作轴；阻断 MCT1 可以增强 Trop-2 ADC（sacituzumab govitecan）和 anti-PD-1 免疫治疗的疗效。

## 文章的核心科学问题

TNBC 对化疗、ADC 和免疫治疗均存在显著异质性。传统 bulk 代谢分析只能看到整个肿瘤组织的平均代谢状态，无法判断某条代谢通路究竟来自肿瘤细胞、髓系细胞、CAF、T 细胞还是其他微环境细胞。该文想解决的问题是：

1. TNBC 微环境中不同细胞类型是否具有不同代谢状态？
2. 这些 cell-type-specific metabolic features 是否比细胞比例或 bulk metabolic score 更能预测治疗反应？
3. 肿瘤细胞与微环境细胞之间是否存在可靶向的代谢互作？
4. 这种代谢互作能否转化为联合治疗策略？

文章的逻辑很清晰：先建图谱，再找治疗相关特征，再提出机制假说，最后用实验和动物模型验证靶点。

## 研究设计与数据

### Discovery cohort

作者建立了一个早期 TNBC 新辅助治疗单细胞队列：

- 共 27 例女性 TNBC 患者。
- 18 例接受 chemotherapy + anti-PD-1 新辅助治疗。
- 7 例接受 chemotherapy alone。
- 另有 2 例纳入代谢分析，但其中 1 例失访，1 例治疗信息缺失。
- 所有样本均为治疗前 diagnostic biopsy。
- 临床反应在手术时评估，主要使用 pCR vs non-pCR。
- 质控后得到 196,294 个细胞，平均每个细胞 1,799 个基因。

### Validation datasets

作者使用多个外部数据进行验证：

- BioKey 人 TNBC scRNA-seq 队列。
- 小鼠 TNBC scRNA-seq 数据。
- I-SPY2 bulk RNA-seq 队列，用于验证 bulk 层面的代谢特征与治疗反应关联。

### 实验验证

文章不止停留在计算分析，而是做了多层实验验证：

- human TNBC tissue multiplex immunofluorescence。
- mouse tumor tissue immunohistochemistry。
- qPCR 验证 4T1 tumor cells 与 BMDM 中转运体表达。
- conditioned medium co-culture。
- GLUT3、MCT4、MCT1 siRNA knockdown。
- 13C-lactate stable isotope labeling。
- MCT1 inhibitor 药物干预。
- xenograft、PDX、4T1 orthotopic、AT3 syngeneic mouse models。
- flow cytometry 分析治疗后免疫微环境变化。

## 方法学框架

### 1. 单细胞预处理与注释

作者使用 Cell Ranger 对 10x scRNA-seq 数据进行比对和定量，参考基因组为 GRCh38。下游分析使用 Seurat v4.3.0。

质控标准：

- UMI/gene number 小于 200 或大于 8000 的细胞被过滤。
- mitochondrial gene percentage < 20%。
- ribosomal gene percentage < 20%。

批次校正与聚类：

- 使用 Harmony 校正患者来源的 batch effect。
- 选择 top 2,000 highly variable genes。
- PCA 降维，选取前 30 个 PCs。
- kNN clustering，resolution = 0.8。
- UMAP 可视化。

主要细胞类型注释 marker：

- T cell：CD2, CD3D, CD3E。
- fibroblast：COL1A1, DCN, C1R。
- myeloid cell：LYZ, CD68, TYROBP。
- cancer cell：CD24, KRT19, SCGB2A2。
- B cell：CD79A, MZB1, MS4A1。
- endothelial cell：CLDN5, FLT1, RAMP2。

### 2. 代谢知识库构建

作者不是只用单一代谢数据库，而是整合 5 个资源建立 metabolic knowledge base：

- KEGG
- Recon3D
- Human-GEM
- Reactome
- BRENDA

这一点很重要，因为单个数据库的代谢基因和通路覆盖有限；整合后可以更系统地从 gene、pathway 和 flux 三个层面分析代谢。

### 3. 三层代谢分析

文章的代谢分析分为三个维度：

1. Metabolic genes：差异表达的代谢基因。
2. Metabolic pathways：代谢通路活性，使用 GSEA、AUCell、scMetabolism 等方式计算。
3. Metabolic fluxes：使用 Compass 进行单细胞代谢通量推断。

### 4. Compass 分析

Compass 使用 Recon2 metabolic network 作为参考，基于 constraint-based modeling 推断单细胞层面的 metabolic reaction potential activity。

作者对每个 reaction 计算 Compass score，然后比较不同细胞类型之间的 reaction activity：

- 使用 Wilcoxon rank-sum test。
- 使用 Cohen's d 作为 effect size。
- p 值用 Benjamini-Hochberg 调整。
- adjusted p < 0.1 被认为 reaction differentially active。

这套思路适合借鉴到我们的转移项目中：不只看某个代谢基因表达，而是把代谢反应、通路和细胞类型结合起来。

### 5. 治疗反应关联分析

作者将每个 major cell type 中的 metabolic pathway score 或 metabolic gene expression 汇总到 patient level，然后用 logistic regression 分析：

`Efficacy (pCR vs non-pCR) ~ Biomarker`

其中 Biomarker 可以是：

- 某细胞类型中的某条代谢通路活性。
- 某细胞类型中的某个代谢基因表达。
- bulk 层面的 pathway score 或 gene expression。

核心思想是：治疗反应预测必须以患者为统计单位，而不是以单细胞为统计单位。

## 主要结果

## Figure 1：研究队列与代谢分析流程

Figure 1 的作用是交代整篇文章的研究设计。

作者建立了一个 TNBC 新辅助治疗前 biopsy scRNA-seq discovery cohort，并纳入外部 validation cohorts。首先进行 major cell type annotation，发现 pCR 和 non-pCR 之间 major cell type proportions 没有显著差异。这是文章后续转向“代谢状态而不是细胞比例”的关键铺垫。

之后，作者整合 KEGG、Recon3D、Human-GEM、Reactome 和 BRENDA，建立 metabolic gene/pathway knowledge base，并从 metabolic genes、metabolic pathways 和 metabolic fluxes 三个层面进行单细胞代谢分析。

### 这一图的叙事作用

Figure 1 不是简单的 workflow 图，而是在建立一个逻辑转折：

细胞比例不能很好解释治疗反应，所以需要进入 cell-type-specific metabolic state。

这对我们的文章有启发：如果我们后续做 Compass，也应该先说明“细胞组成和 CoVarNet 模块解释了哪些细胞共同出现，但还不能解释这些模块在不同器官中承受怎样的代谢压力”，然后自然引出代谢分析。

## Figure 2：TNBC 微环境中不同细胞类型具有明显代谢异质性

作者先从整体角度观察不同细胞类型的代谢差异。无论使用 metabolic genes、metabolic pathways 还是 metabolic fluxes，t-SNE/UMAP 都能将不同细胞类型分开，说明 TNBC 微环境中的代谢状态具有明显细胞类型特异性。

关键结果：

- Tumor cells 具有最高的 metabolic gene expression。
- Pathway activity 层面，tumor cells 最高，其次是 CAFs、myeloid cells 和 endothelial cells。
- Compass flux 层面，tumor cells 仍表现出最高整体代谢活性。
- Myeloid cells 和 CAFs 在 flux 层面也有较高活性，因此值得进一步研究。

### 生物学意义

这个结果强调：代谢重编程不是肿瘤细胞独有的现象，微环境细胞也存在强烈代谢活性和代谢适应。尤其是 CAF 和 myeloid cells，它们可能通过代谢改变参与治疗抵抗。

### 方法学意义

单细胞代谢分析必须按细胞类型拆开做。bulk metabolic activity 容易被肿瘤细胞比例或高表达细胞群主导，掩盖微环境中真正有功能意义的代谢状态。

## Figure 3：不同细胞类型具有不同代谢偏好

作者进一步解析每类细胞的具体代谢特征。

### Tumor cells

肿瘤细胞表现为：

- nutrient metabolism 增强。
- oxidative stress protection 增强。
- MGST1 和 GSTP1 高表达。
- glycolysis/gluconeogenesis 增强。
- glutathione-associated metabolism 增强。

解释：肿瘤细胞快速增殖，需要大量生物合成和能量供应，同时还要抵御氧化应激，因此依赖糖酵解、谷胱甘肽代谢等保护性代谢程序。

### Myeloid cells

髓系细胞表现为：

- APOE、APOC1 等脂质代谢相关基因上调。
- lipid metabolism and peroxidation 增强。
- fatty acid oxidation、glutamate metabolism、unsaturated fatty acid biosynthesis 增强。
- ferroptosis-associated pathways 活跃。

解释：TNBC 中髓系细胞可能处在脂质代谢和氧化压力重塑状态，这可能与免疫抑制、吞噬功能、炎症状态或治疗反应相关。

### CAFs

CAF 表现为：

- proline metabolism 增强。
- lysine metabolism 增强。
- NNMT 特异表达。
- 与 extracellular matrix remodeling 相关。

解释：脯氨酸和赖氨酸与胶原、ECM 合成和基质重塑关系密切。CAF 代谢状态与 ECM remodeling 连接，这是肿瘤进展和治疗抵抗的重要机制。

### T cells

T 细胞表现为：

- DUSP2、GZMA 高表达。
- fatty acid oxidation 中度增强。

解释：T 细胞的脂质代谢重编程可能与抗肿瘤免疫功能、效应/记忆状态或耗竭状态相关。

### B cells

B 细胞表现为：

- CD38 等代谢相关基因表达。
- 脂质代谢、氨基酸代谢和碳代谢 flux 相对较低。

### Endothelial cells

内皮细胞表现为：

- FABP4 等脂质代谢相关基因表达升高。
- lipid metabolism flux 增强。

### 结果可靠性

作者用 BioKey 队列和 mouse TNBC scRNA-seq 数据验证了关键结果，说明单细胞代谢特征不是 discovery cohort 特有现象。

## Figure 4：细胞类型特异性代谢特征优于细胞比例和 bulk 代谢特征预测治疗反应

这是文章中非常关键的一步。作者发现 major cell type proportions 在 pCR 和 non-pCR 之间没有明显差异，但不同细胞类型中的 metabolic activities 与治疗反应具有明显关联。

### 主要结果

在 immune cells 中：

- 多数关键代谢通路活性与 chemotherapy 和 chemo-immunotherapy 后 pCR 正相关。
- 说明免疫细胞的代谢活跃状态可能支持更有效的抗肿瘤免疫或治疗敏感性。

在 CAFs 中：

- nucleotide metabolism、amino acid metabolism、carbohydrate metabolism 更活跃时，提示治疗反应较差。
- 说明 CAF 的代谢活跃状态可能支持肿瘤耐药、基质屏障或免疫抑制。

在 tumor cells 中：

- 不同氨基酸代谢方向不一致。
- arginine biosynthesis 和 D-glutamine/D-glutamate metabolism 活性较高与更好反应相关。
- tyrosine metabolism 和 glutathione metabolism 活性较高则预测 chemo-immunotherapy 后 non-pCR。

在 T cells 中：

- FBP1 表达较高与 chemo-immunotherapy 后 pCR 相关。

在 CAFs 中：

- APRT 等 purine metabolism genes 高表达提示 chemo-immunotherapy 后较差反应。

### 与 bulk 分析对比

作者进一步在 I-SPY2 bulk RNA-seq 中分析代谢通路和代谢基因的预测能力，但发现只有少数 pathway 或 gene 有预测价值。更重要的是，bulk glutathione metabolism 的趋势甚至可能与特定细胞群分析相反。

这说明：

1. bulk metabolic score 可能混合了多个细胞类型的相反信号。
2. 同一代谢通路在不同细胞类型中可能具有不同临床意义。
3. cell-type-specific metabolic activity 比 cell fraction 或 bulk metabolism 更接近真实生物机制。

### 对我们项目的直接启发

后续在乳腺癌转移中做 Compass 时，不应只报告“某个转移器官糖酵解高”或“脂肪酸代谢高”。更有价值的写法是：

- 肺转移 CM06-high 样本中，Mph-MARCO 或 cDC2 的脂质/氧化应激/乳酸相关代谢是否增强？
- 脑转移 Brain CM-high 样本中，Mph-HSPA1A/GPNMB/ISG15 或 BBB-like endothelium 的 stress metabolism、glycolysis、tryptophan metabolism 是否增强？
- 卵巢转移 CM05-high 样本中，Mph-SPP1/FOLR2 和 Bm 的 lipid/cholesterol/purine metabolism 是否增强？
- 胸壁 CM04-high 样本中，CAF/ECM/Treg/Mph-MKI67 的 proline/lysine/collagen-associated metabolism 是否增强？

核心是：代谢分析必须围绕 cell module 和 module-defining cell types 展开。

## Figure 5：肿瘤细胞与髓系细胞存在不同葡萄糖利用和乳酸生成模式

在观察到 cancer cells 和 myeloid cells 具有较高代谢活性后，作者聚焦 central carbon metabolism，尤其是 glucose utilization 和 lactate production。

### Tumor cells 的代谢特征

肿瘤细胞中：

- PFKP 表达升高。
- LDHB 表达升高。
- OGDHL 表达升高。
- glycolysis flux 增强。
- TCA cycle flux 增强。
- MCT4/SLC16A3 表达较低。
- lactate secretion flux 较低。

作者据此提出：肿瘤细胞除了利用葡萄糖来源的碳进入 TCA cycle，可能还需要直接利用现成乳酸作为碳源，以支持快速增殖。

### Myeloid cells 的代谢特征

髓系细胞中：

- GLUT3/SLC2A3 上调。
- MCT4/SLC16A3 上调。
- glucose uptake flux 增强。
- lactate secretion flux 增强。

这说明髓系细胞可能是 TNBC 微环境中强葡萄糖摄取和乳酸输出的细胞群。

### 外部验证和临床关联

作者在 BioKey 和 mouse TNBC scRNA-seq 数据中验证了 GLUT3/MCT4/MCT1 的表达模式。

同时，GLUT3 和 MCT4 高表达趋势上指向更差的 chemotherapy 和 immunotherapy response。将髓系细胞 GLUT3/MCT4 高表达和肿瘤细胞 MCT1 高表达结合后，可以构建一个代谢互作风险模型。

### 机制模型

作者提出：

1. Myeloid cells 通过 GLUT3 摄取葡萄糖。
2. Myeloid cells 通过 MCT4 输出乳酸。
3. Tumor cells 通过 MCT1 摄取髓系细胞释放的乳酸。
4. Tumor cells 将乳酸重新用于 central carbon metabolism 和 TCA cycle。
5. 这种 lactate re-use cycling 促进肿瘤生长和治疗抵抗。

这个模型的本质不是“肿瘤细胞产生乳酸抑制免疫”这一经典 Warburg 叙事，而是“髓系细胞产生乳酸，肿瘤细胞回收乳酸”的反向或协同代谢循环。这是本文最有新意的机制点。

## Figure 6：GLUT3/MCT1/MCT4 轴介导肿瘤-髓系乳酸循环并促进 TNBC 进展

Figure 6 是机制验证。

### 表达定位验证

作者通过多种方式确认：

- GLUT3 和 MCT4 主要表达于 myeloid cells。
- MCT1 主要表达于 tumor cells。

验证方法包括：

- human TNBC multiplex immunofluorescence：CD11b、MPO、GLUT3、MCT1、MCT4。
- mouse 4T1 tumor tissue IHC：CD11b、MCT1、MCT4、GLUT3。
- 4T1 tumor cells 与 BMDMs 的 qPCR。

### 共培养功能验证

作者使用 conditioned medium co-culture：

- 在 myeloid cells 中敲低 GLUT3 或 MCT4，会阻断乳酸产生或输出，并抑制肿瘤细胞生长和迁移。
- 在 tumor cells 中敲低 MCT1，会阻断乳酸摄取，并抑制肿瘤细胞进展。

### 13C 乳酸示踪

作者进行 13C-lactate stable isotope labeling，证明敲低 MCT1 会降低肿瘤细胞对 13C-lactate 的摄取，并减少 13C 标记进入 glycolysis 和 TCA cycle 中间代谢物。

这一步很关键，因为它从“表达相关”推进到了“代谢流动证据”。它证明 MCT1 不是只在肿瘤细胞上表达，而是确实参与乳酸输入和碳源利用。

### 药物验证

MCT1 inhibitor 在体内和体外均产生类似效果，降低乳酸循环和肿瘤进展。

### 机制总结

GLUT3/MCT4-high myeloid cells 与 MCT1-high tumor cells 形成 lactate re-use cycling。髓系细胞不仅是免疫调节细胞，也可作为代谢供体，为肿瘤细胞提供可再利用的乳酸碳源。

## Figure 7：MCT1 inhibitor 增强 ADC 和免疫治疗疗效

Figure 7 是临床转化验证，也是文章的落脚点。

### 与 sacituzumab govitecan 联合

Sacituzumab govitecan（SG）是 Trop-2 ADC，在 TNBC 中具有重要治疗价值。作者测试 MCT1 inhibition 是否能增强 SG 效果。

结果：

- tumor-cell MCT1 knockdown 增强 SG 抗肿瘤效果。
- myeloid-cell MCT4 knockdown 也增强 SG 效果。
- MCT1 inhibitor + SG 比单药更显著降低肿瘤负荷。
- PDX 模型中也验证了 MCT1 inhibitor 对 SG 的增敏作用。

### 与 anti-PD-1 联合

作者在 4T1 orthotopic TNBC 模型中测试 MCT1/MCT4 抑制与 ICB 的联合效果。

结果：

- tumor-cell MCT1 suppression 或 myeloid-cell MCT4 suppression 均增强 anti-PD-1 效果。
- MCT1 inhibitor + anti-PD-1 比任一单药更强地抑制肿瘤。
- AT3 syngeneic TNBC 模型中得到类似结果。

### 免疫微环境变化

MCT1 inhibitor + anti-PD-1 联合治疗后：

- CD4+ T cells 增加。
- CD8+ T cells 增加。
- PRF+ CD8+ cytotoxic T cells 增加。
- GZMB+ CD8+ T cells 在 AT3 模型中增加。
- CD86+ macrophages 增加。
- PD-1+ CD8+ T cells 减少。
- CD206+ macrophages 减少。

这说明 MCT1 inhibition 不只是直接抑制肿瘤细胞代谢，还可能重塑免疫微环境，使其更有利于抗肿瘤免疫。

## 文章最终模型

这篇文章的机制模型可以概括为：

```text
TNBC microenvironment
        |
        v
Myeloid cells:
GLUT3 high -> glucose uptake
MCT4 high -> lactate export
        |
        v
Extracellular lactate pool
        |
        v
Tumor cells:
MCT1 high -> lactate import
lactate enters glycolysis/TCA intermediates
        |
        v
Tumor growth, migration, therapy resistance
        |
        v
MCT1 inhibition disrupts lactate re-use
        |
        v
Enhanced response to SG and anti-PD-1
```

与经典 Warburg effect 的差异在于：

- 经典模型常强调 tumor cells 产生乳酸并抑制免疫细胞。
- 本文模型强调 myeloid cells 可以高效摄取葡萄糖并输出乳酸，而 tumor cells 通过 MCT1 摄取乳酸并作为碳源重新利用。

这种“代谢互惠”或“代谢分工”是文章的核心创新点。

## 文章叙事逻辑

文章的叙事非常值得借鉴，结构如下：

1. 临床问题：TNBC 对化疗、ADC 和免疫治疗反应异质性大，缺少有效增敏策略。
2. 知识缺口：bulk metabolism 不能解析 TME 中不同细胞类型的代谢状态和互作。
3. 数据资源：建立治疗前 TNBC biopsy scRNA-seq 队列，并结合外部队列验证。
4. 方法升级：从 metabolic genes、pathways、fluxes 三个层面构建单细胞代谢图谱。
5. 现象发现：不同细胞类型具有显著代谢异质性，细胞比例本身不能预测 pCR。
6. 临床关联：cell-type-specific metabolic features 比 cell fraction 和 bulk metabolism 更能关联治疗反应。
7. 机制聚焦：从全局代谢图谱中锁定 tumor-myeloid glucose-lactate crosstalk。
8. 功能验证：mIF、IHC、qPCR、co-culture、knockdown、13C tracing 验证 GLUT3/MCT4/MCT1 轴。
9. 治疗转化：MCT1 inhibitor 增强 SG 和 anti-PD-1 疗效。
10. 临床意义：MCT1 是一个可药物化、可联合治疗的代谢靶点。

这个叙事强在“从大图谱到小机制，再到治疗验证”。它没有停留在 omics 描述，而是把单细胞发现转化成可干预靶点。

## 文章创新点

### 1. 将 TNBC TME 代谢分析推进到细胞类型分辨率

以往研究更多关注 bulk tumor metabolism 或某些代谢酶表达。该文强调同一代谢通路在不同细胞类型中可能具有完全不同的临床意义。

### 2. 同时使用 gene、pathway 和 flux 三个维度

单看代谢基因表达可能不能代表真实代谢活性。作者整合 metabolic gene expression、pathway activity 和 Compass flux prediction，使代谢状态解析更立体。

### 3. 证明 cell-type-specific metabolic traits 优于 cell proportions

文章先证明 pCR 与 non-pCR 的 major cell type fractions 没有显著差异，再证明细胞类型特异性代谢状态能更好关联治疗反应。这一逻辑很有说服力。

### 4. 提出 myeloid-to-tumor lactate re-use cycling

这是本文最关键的机制创新。髓系细胞不只是免疫抑制细胞，也可作为代谢供体；肿瘤细胞不只是乳酸生产者，也可以是乳酸消费者。

### 5. 给出可转化治疗策略

MCT1 inhibitor AZD3965 已经完成 phase I clinical trial，具有一定临床可及性。作者用 ADC 和 ICB 两个方向证明其联合治疗潜力。

## 文章局限

作者自己也承认了几个局限：

1. 代谢推断主要基于 scRNA-seq，转录水平不能完全代表代谢流。
2. 尽管有外部队列和代谢实验支持，仍需要 multi-omics 和更多功能实验验证。
3. 样本量有限，某些治疗反应和细胞群贡献需要更大队列验证。
4. 小鼠模型主要评估 tumor growth 和 TME alterations，尚未充分验证 survival outcomes。
5. MCT1 inhibitor 虽然主要针对肿瘤细胞高表达 MCT1，但不能排除其作用于其他微环境细胞。

对我们来说，最需要吸取的是：Compass 或 scRNA 代谢分析本质上是推断，最好与代谢基因、通路、LR、空间邻近、临床关联或外部数据交叉验证。

## 对我们乳腺癌转移项目的启发

这篇文章对我们后续做“转移器官特异性代谢重编程”非常有参考价值。

### 1. 代谢分析要服务于 CoVarNet 模块，而不是另起炉灶

我们目前的文章主线是：

```text
paired primary-metastasis atlas
-> immune composition and cell-state remodeling
-> CoVarNet organ-specific cellular modules
-> LR/CellChat communication circuits
-> clinical/external validation
-> metabolic constraints by Compass
```

代谢分析应该作为解释 CoVarNet module 形成机制的延伸，而不是单独写成“转移灶代谢图谱”。可借鉴本文的逻辑：

1. 先证明不同转移器官的 module-defining cells 具有不同代谢状态。
2. 再证明 cell-type-specific metabolism 与 CM activity 相关。
3. 再锁定某些细胞之间的 metabolic crosstalk。
4. 最后将代谢轴与 LR 通讯轴、临床进展或外部数据相联系。

### 2. 分析单位应是 cell type × organ × module

不要只比较“肺转移 vs 脑转移整体代谢”。更合理的是：

- Lung CM06-high 的 Mph-MARCO/cDC2/CD8_Temra 代谢状态。
- Brain CM-high 的 Mph-HSPA1A/GPNMB/ISG15、BBB-like endothelial、plasma cell 代谢状态。
- Ovarian CM05-high 的 Bm、Mph-SPP1、Mph-FOLR2 代谢状态。
- ChestWall CM04-high 的 Mph-MKI67、CAF、Treg、mast/endothelial 代谢状态。

这样才能避免 bulk-like averaging。

### 3. 关键分析可以这样设计

对每个转移器官：

1. 提取 module-defining cell types。
2. 对这些细胞运行 metabolic gene score、pathway score 和 Compass reaction score。
3. 计算每个样本、每个细胞类型的 metabolic pathway activity。
4. 将 metabolic activity 与 CM activity 做 Spearman correlation。
5. 将 metabolic activity 与 module-associated cell fraction 做相关。
6. 将 metabolic activity 与 focused LR score 做相关。
7. 若有配对样本，计算 metastasis vs primary 的 paired metabolic shift。

可以形成一个三联证据：

```text
CM activity high
    |
    +-- module-defining cells enriched
    +-- module-defining LR axes stronger
    +-- module-defining metabolic pathways activated
```

### 4. 四个器官可提出不同代谢压力模型

#### 肺转移

肺 CM06 是 Mph-MARCO/cDC2/CD8_Temra 生态，LR 轴涉及 ICAM1、CXCL12、C5、PVR、THBS1/CD36。

可重点看：

- fatty acid metabolism
- lipid uptake/scavenger receptor-related metabolism
- oxidative phosphorylation
- glutathione metabolism
- complement-related inflammatory metabolism
- lactate transporters: MCT1/SLC16A1, MCT4/SLC16A3
- CD36-linked fatty acid uptake

假说：肺转移 CM06-high 可能是脂质处理、氧化应激和黏附-髓系活化共同塑造的 immune-infiltrated but restrained 生态。

#### 脑转移

脑转移模块与 BBB-like/IFN endothelium、Mph-HSPA1A/GPNMB/ISG15、CD4/plasma cell 相关。

可重点看：

- glycolysis
- oxidative stress response
- tryptophan metabolism
- NAD metabolism
- lactate metabolism
- mitochondrial stress
- amino acid metabolism under BBB nutrient limitation

假说：脑转移中的血管屏障和 IFN/stress microenvironment 可能驱动 macrophage 和 plasma cell 的应激代谢状态，并通过 CXCL10/CXCL12/ICAM1/LGALS9 等轴形成免疫浸润但受限的脑转移生态。

#### 卵巢转移

卵巢 CM05 是 Bm-Mph-SPP1/FOLR2 生态，LR 轴涉及 APP-CD74、MIF-CD74/CXCR4、BAFF-BAFFR、THBS1-CD36/CD47。

可重点看：

- lipid metabolism
- cholesterol metabolism
- fatty acid oxidation
- purine metabolism
- amino sugar metabolism
- oxidative phosphorylation
- macrophage phagocytosis/scavenger-related metabolism

假说：卵巢转移可能依赖 B cell-macrophage survival/retention niche，并受到腹腔/浆膜样脂质代谢环境和 CD74/BAFF 轴共同塑造。

#### 胸壁复发/转移

胸壁 CM04 是 ECM-rich、Mph-MKI67、Treg、mast、CAF 相关生态，LR 轴涉及 COL4A1/2-SDC4、CXCL12-CXCR4、CCL22-CCR4、LTA-TNFRSF、THY1/integrins。

可重点看：

- arginine and proline metabolism
- lysine metabolism
- collagen biosynthesis-related pathways
- glycolysis
- hypoxia-related metabolism
- oxidative phosphorylation
- nucleotide synthesis in proliferating macrophages

假说：胸壁复发/转移是 tissue repair-like metabolic niche，CAF/ECM 代谢、胶原相关氨基酸代谢和增殖型巨噬细胞代谢共同支持局部复发。

## 可以直接借鉴的写作逻辑

### 代谢章节标题建议

Metabolic inference links organ-specific multicellular modules to target-organ-imposed metabolic constraints

或：

Cell-type-specific metabolic rewiring underlies organ-dependent metastatic immune modules

### 代谢章节核心段落

可写为：

To determine whether organ-specific cellular modules were associated with distinct metabolic constraints, we inferred metabolic pathway and reaction activities in module-defining cell populations using cell-type-resolved metabolic scoring and Compass. Rather than analyzing bulk-like metabolic signatures, we summarized metabolic activities at the sample and cell-type levels and correlated them with organ-specific CoVarNet module activity. This strategy allowed us to identify metabolic programmes selectively associated with coordinated metastatic immune ecosystems.

中文解释：

为了判断器官特异性细胞模块是否伴随不同代谢压力，我们在模块定义细胞群中推断代谢通路和代谢反应活性，并将其汇总到样本-细胞类型水平，再与 CoVarNet module activity 相关。该策略避免了 bulk-like 代谢信号混杂，可以解析特定细胞状态在特定转移器官中的代谢适应。

### 与本文对应的叙事迁移

本文逻辑：

```text
cell-type proportions cannot explain pCR
-> cell-type-specific metabolism improves prediction
-> tumor-myeloid lactate crosstalk
-> MCT1 inhibitor sensitizes therapy
```

我们的逻辑可以是：

```text
major cell types alone cannot explain organ-specific metastatic immune ecology
-> CoVarNet identifies organ-specific multicellular modules
-> cell-type-specific metabolism explains module-specific constraints
-> metabolic activity links with LR circuits and clinical progression
```

这会让代谢分析自然接入文章主线。

## 这篇文章最值得学习的地方

### 1. 先证明传统指标不够，再引入新方法

作者先证明 cell-type proportions 不能区分 pCR/non-pCR，再引出 metabolic feature 的必要性。我们也可以先说明单个细胞比例不足以解释转移器官差异，再引出 CoVarNet 多细胞协同模块。

### 2. 从全局图谱中收敛到一个机制轴

全文不是把所有代谢结果都机械列出，而是从大量结果中聚焦到 GLUT3/MCT4/MCT1 lactate axis。我们做转移项目时，也应避免每个器官列几十条代谢通路，而要为每个器官收敛到 1-2 个机制假说。

### 3. 机制轴必须能连接细胞类型

本文的关键轴不是单细胞内的代谢基因，而是跨细胞互作：

- myeloid cells：GLUT3/MCT4
- tumor cells：MCT1
- metabolite：lactate
- phenotype：tumor progression and therapy resistance

我们的代谢分析也应寻找类似结构：

- sender cell metabolic state
- receiver cell metabolic dependency
- metabolite or transporter
- LR/communication axis
- CM activity and clinical phenotype

### 4. 多层验证增强说服力

本文使用外部 scRNA、bulk cohort、mIF、IHC、qPCR、co-culture、13C tracing、小鼠模型和 PDX 层层验证。我们未必能做到功能实验，但至少应做到：

- 内部样本相关。
- 配对原发-转移验证。
- 外部单细胞验证。
- 空间邻近验证。
- bulk signature/clinical validation。

## 对我们文章的具体分析清单

### 必做

- 对四个转移器官的 module-defining cell types 计算 metabolic pathway activity。
- 对这些细胞运行 Compass，得到 reaction/pathway level flux score。
- 计算 metabolic activity 与 CM activity 的 Spearman correlation。
- 计算 metabolic activity 与 focused LR score 的相关。
- 做配对原发-转移的 metabolic shift。

### 强烈建议

- 对每个器官选择 2-3 条最有解释力的代谢通路，而不是铺开全部结果。
- 将代谢结果与已识别 LR 轴放在同一个机制模型中。
- 对关键 transporter/metabolic genes 做 dotplot 或 heatmap，例如 SLC16A1/MCT1、SLC16A3/MCT4、SLC2A3/GLUT3、LDHA/LDHB、CD36、APOE、APOC1、NNMT、GSTP1、MGST1。
- 在 external scRNA 或 spatial 数据中验证关键代谢基因是否由相同细胞群表达。

### 可选

- 如果有治疗或转移时间数据，将 organ-specific metabolic module score 与 early/late metastasis 关联。
- 如果有 bulk 队列，用 module-defining metabolic signature 做 survival 或 metastasis-free survival。
- 如果有空间数据，验证代谢高表达细胞是否与 LR sender/receiver 细胞空间邻近。

## 可用于 Discussion 的启发句

This study by Xu et al. provides an important conceptual framework for interpreting cancer metabolism at cell-type resolution. Their finding that myeloid-derived lactate can be reutilized by MCT1-high tumor cells illustrates that metabolic reprogramming in cancer is not merely a tumor-cell-intrinsic process but can emerge from coordinated interactions between malignant and microenvironmental compartments. In the context of breast cancer metastasis, this framework suggests that organ-specific immune modules may also be shaped by metabolic exchange and nutrient constraints imposed by local tissue environments.

中文版本：

Xu 等人的研究提供了一个重要概念框架：肿瘤代谢重编程并不只是肿瘤细胞内在事件，而可以由恶性细胞与微环境细胞之间的代谢分工和物质交换共同产生。其发现的髓系细胞来源乳酸被 MCT1 高表达肿瘤细胞重新利用的机制，提示在乳腺癌转移中，不同器官特异性的免疫细胞模块也可能受到局部组织营养限制、代谢压力和细胞间代谢互作的共同塑造。

## 结论

这篇文章的价值不只是发现 MCT1 可以增敏 ADC 和免疫治疗，更重要的是提供了一个完整研究范式：通过单细胞数据识别细胞类型特异性代谢状态，再将其与治疗反应关联，从中收敛出跨细胞代谢互作轴，并进一步用功能实验和动物模型验证靶点。

对我们的乳腺癌转移项目而言，最值得借鉴的是“cell-type-specific metabolic features”这一思想。我们不应把代谢分析写成泛泛的器官差异，而应围绕 CoVarNet 识别出的肺、脑、卵巢和胸壁转移模块，解析各模块核心细胞在原发到转移过程中如何发生代谢重编程，以及这些代谢状态如何与 LR 通讯轴、器官微环境和临床进展相互耦合。
