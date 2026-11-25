# 文档目录 / Document Outline

版本：0.1 · Phase 0 讨论稿

结构以实际五大页面为主线，保持英文导航顺序 Inspect → Recipe → PLC → Records → Cameras。以下为完整的初始二、三级目录，并非已核实的最终功能清单。每个编号作为稳定的协作与素材引用 ID，后续可增删。章节标题中英文暂定，实际界面词汇由 Phase 1 核对。

状态约定：核心内容默认纳入；标有「可选」的内容需确认实用性与成熟度；标有「讨论」的内容待对应小节决定。素材引用见 ASSET_MANIFEST.md。

## 01 软件概述 / Overview

### 1.1 软件简介 / Introduction
- 1.1.1 应用与主要能力 / Application and Capabilities：多通道胶囊视觉检测、生产与记录、配方与模型、相机与通信。30/36 通道；72 通道设备是否作为双独立 IPC 的部署实例展示待核实，不描述跨 IPC 同步。
- 1.1.2 基本检测流程 / Inspection Workflow：图像采集 → 视觉检测与规则 → 结果输出 → 记录；机械和电气只作为接口上下文。素材 G-01。

### 1.2 界面与导航 / Interface and Navigation
- 1.2.1 五大页面 / Five Workspaces：各页用途及全局状态、当前配方、用户菜单等基本入口。素材 UI-01。
- 1.2.2 用户与权限 / Users and Access：常见角色、权限差别、登录与切换；不展开安全实现及离线恢复。
- 1.2.3 显示与语言 / Display and Language：英文界面作为手册参考，主题、语言选择与多语言支持。素材 UI-02（必要时）。

### 1.3 开始使用 / Getting Started
- 1.3.1 基本准备与日常使用顺序 / Basic Preparation and Daily Workflow：已配置设备的登录、批次、配方、启动；指向第 2 章与附录 A，不重复步骤。
- 1.3.2 阅读指南 / How to Use This Guide（可选）：以读者任务定位章节；可压缩为一段。

## 02 生产检测 / Inspect

### 2.1 生产界面 / Production Workspace
- 2.1.1 实时图像与状态 / Live Images and Status：相机卡片、通道结果、运行状态、信息栏。素材 UI-03。
- 2.1.2 检测通道与相机 / Channels and Cameras：每相机六个通道，通道与图像位置关系。素材 IN-01。

### 2.2 检测过程与结果 / Inspection Process and Results
- 2.2.1 双光学视图与多次曝光 / Dual Optical Views and Multiple Exposures：上下视图来自同一胶囊；正常四次曝光流程的作用，不将观察角度描述为已标定的完整表面覆盖。素材 IN-02、G-02。
- 2.2.2 检测规则概览 / Inspection Rules at a Glance：Occupancy、模型缺陷、Length、Color，详细原理放第 3 章。素材 G-03。
- 2.2.3 结果合并与指示 / Result Merging and Indicators：正常四次曝光的拒绝请求合并；指示灯与第四次图像、OK/NG/EMPTY、技术异常的区别。素材 IN-02、UI-04。

### 2.3 日常生产操作 / Daily Production Operations
- 2.3.1 选择配方 / Selecting a Recipe：选择已保存配方，准备状态；带已设置工艺覆盖项的配方选择可能写入 PLC，需按设备停止条件操作。
- 2.3.2 批次设置 / Batch Setup：新建、继续、结束批次及产品信息，避免误用既有批号。素材 UI-05。
- 2.3.3 启动与停止 / Start and Stop：启动准备、等待运行状态、外部触发、正常停止。PLC 组模式按已知新组边界启动；软件 Stop 不是机械停止。素材 UI-06。

### 2.4 生产统计 / Production Statistics
- 2.4.1 周期、组与批次 / Cycles, Groups and Batches：区分计数单位与常用模式，避免将四次曝光当作四粒胶囊。素材 G-04。
- 2.4.2 合格数量与合格率 / Counts and Pass Rate：组模式位置统计、空位与实际质量验证之间的关系；详细报告见第 5 章。

## 03 产品配方 / Recipe

本章为主要技术章节。先介绍配方及其实际工作流，再根据具体功能引入视觉原理与第三方训练工具。与 Records 的效果分析、Cameras 的成像质量互相引用。

### 3.1 配方管理 / Recipe Management
- 3.1.1 配方与设备配置 / Recipe and Equipment Configuration：配方保存产品规则，ROI/相机映射属于设备层。素材 UI-07、G-05。
- 3.1.2 新建、复制与命名 / Create, Duplicate and Name：建议优先从已验证的相似配方复制。
- 3.1.3 保存、重新选择与应用 / Save, Reselect and Apply：Save → Inspect 重新选择准备 → Start 生效；工艺覆盖值另在选择后应用、读回，具体条件见 3.7。素材 G-06、UI-08。

### 3.2 模型管理 / Model Management
- 3.2.1 模型是什么 / What Is a Model?：已训练模型的功能与使用条件，不宣传供应商。
- 3.2.2 模型导入与选择 / Importing and Selecting a Model：实际模型文件、正确模型选择、标签显示、常用更新。素材 UI-09。
- 3.2.3 模型适用性与变更 / Model Suitability and Updates：产品颜色、材质、图像分布、缺陷目标；新模型需重新验证。模型删除仅有实际必要时简述。

### 3.3 深度学习图像分割 / Deep Learning Image Segmentation
- 3.3.1 分类、目标定位与分割 / Classification, Localization and Segmentation：用简图建立区别；本软件使用分割结果。素材 G-07。
- 3.3.2 像素、Mask 与外接矩形 / Pixels, Masks and Bounding Boxes：分割面积和框面积的差别，单位 px²。素材 IN-03。
- 3.3.3 从全图推理到通道判定 / From Full-image Inference to Channel Decisions：全相机推理、ROI 归属、标签、面积、存在性筛选；不把分割原始输出等同于最终 NG。素材 G-03、IN-01、IN-03。

### 3.4 缺陷定义与模型训练 / Defect Definition and Model Training
- 3.4.1 缺陷定义和检测目标 / Defect Definition and Inspection Objectives（讨论）：依据产品实际质量标准确定应识别的缺陷、容许现象与边界样本；不替客户制定通用验收标准。素材 IN-05。
- 3.4.2 标签体系设计 / Designing Defect Labels（讨论）：同一标签内语义一致、类别之间尽量可区分；考虑分类统计与实际处理需求，避免无必要的过细/混合标签。名称用真实已确认标签举例。
- 3.4.3 标注准则 / Annotation Guidelines（讨论）：像素范围、边界、细小缺陷、反光/阴影、模糊/歧义样本、多缺陷样本与一致性审阅。用真实正反例制定准则，未定义的标签不擅自纳入正式标准。素材 TR-02、TR-03。
- 3.4.4 数据采集与划分 / Data Collection and Dataset Split：覆盖颜色、规格、批次、不同通道/姿态及成像变化；按物理胶囊或相关采集批次合理划分，避免同一胶囊的多张高度相关图像跨训练/验证集导致结果偏乐观。素材 IN-02、TR-01。
- 3.4.5 自动标注、人工复核与自动训练 / Assisted Annotation, Review and Automated Training：用当前商业工具实际截图展示可用流程，自动化降低操作工作量，但仍需确认缺陷语义、标注一致性和训练结果。实际功能与名称待实机核对。素材 TR-04、TR-05。
- 3.4.6 模型验证与迭代 / Model Validation and Iteration：独立样本、误检/漏检、难例补充、比较新旧模型；面向使用，不讲数学推导或未公开网络架构。素材 TR-06、IN-06。
- 3.4.7 导出与部署 / Export and Deployment：第三方工具输出模型文件，回到 3.2 导入与 3.1 应用；精确操作待工具版本确认。素材 TR-07。

### 3.5 缺陷类别及面积过滤 / Defect Labels and Area Filtering
- 3.5.1 类别启用 / Label Selection：选用当前模型返回的真实缺陷标签；禁用类别不等于改变模型本身。
- 3.5.2 最小 Mask Area / Minimum Mask Area：模型原生输出门限与配方后过滤的区别；等于门限有效；按类别设定、0/空白表示不增加面积过滤。当前算法在每曝光、通道、视图、类别保留相应代表区域。素材 UI-10、IN-04。
- 3.5.3 面积设置与效果验证 / Area Adjustment and Verification：真缺陷与误检案例对照，解释过高阈值的漏检风险；像素面积与物理面积不同，未经标定不换算成 mm²。素材 IN-04、IN-06。

### 3.6 存在性、长度与颜色 / Occupancy, Length and Color
- 3.6.1 存在性检测 / Occupancy：自动检测胶囊存在/空位，对其他规则的影响。说明工作原理，不罗列内部固定常数。素材 IN-07。
- 3.6.2 长度检测与调节 / Length Inspection and Tuning：图像纵向像素长度、目标值、允许偏差、参考图像及采样。素材 UI-11、IN-08。
- 3.6.3 颜色检测与调节 / Color Inspection and Tuning：HSV 与循环色相、目标色相、允许偏差、参考图像；颜色结果依赖成像一致性。素材 UI-12、G-08、IN-09。
- 3.6.4 调试与生产的区别 / Tuning and Production：冻结参考图、当前单项预览、保存与实际生产验证的关系；不把单张调试结果当作完整生产判定。

### 3.7 工艺参数与图像存档 / Process Parameters and Image Archive
- 3.7.1 图像存档 / Image Archive：保存目录、OK/NG/ERROR/EMPTY；正常四曝光模式下 NG 及相应面积过滤证据的组内留图条件。素材 UI-13。
- 3.7.2 工艺参数 / Process Parameters：如设备使用此功能，解释覆盖项、空白与零、保存和重新选择应用的区别、状态提示；不展开寄存器/机械调节值。（实际现场使用方式待核实。）素材 UI-14。

### 3.8 新规格验证 / Validating a New Product Recipe
- 3.8.1 配方设置顺序 / Setup Sequence：依据已批准的缺陷目标与相似配方、模型、尺寸/颜色与存档配置。
- 3.8.2 样品验证与优化 / Sample-based Verification：检查真实良品、缺陷品、典型难例，多通道多视图覆盖，结合实际效果调整参数；与第 5 章及附录 B 配合，不给出无依据的统一合格阈值。

## 04 设备通信 / PLC

### 4.1 通信界面与连接 / Communication Workspace and Connections
- 4.1.1 页面组成 / Workspace Overview：Reporting、Status、Diagnostics、Web 等实际常用部分。素材 UI-15。
- 4.1.2 网络参数与协议 / Network Settings and Protocols：IP、端口、协议与实际设备配置的匹配；仅介绍已收录模式，不列完整报文或寄存器表。

### 4.2 检测结果与设备状态 / Reporting and Equipment Status
- 4.2.1 结果发送 / Result Reporting：检测结果与 PLC 接收，常用合并模式、状态观察、设置的启用时机；不介绍 PLC 程序或机械控制逻辑。素材 G-09、UI-16。
- 4.2.2 报警与通信状态 / Alarms and Communication Status：独立状态服务、报警出现/恢复、通信不可用与实际无报警的区别。素材 UI-17。
- 4.2.3 常用连接检查 / Basic Connection Checks：核对配置、观察状态和反馈；若现场有常用只读诊断需求，经验证再展示。（可选）

### 4.3 其他常用功能 / Additional Common Functions
- 4.3.1 Web 观察 / Web Observer（可选）：确实常用时介绍启用和只读浏览，在受控局域网使用。素材 UI-18。
- 4.3.2 工艺参数与设备通信 / Process Parameters and Communication：如纳入 3.7.2，简要说明设备目标与结果确认，交叉引用，不重复。

暂不介绍手动寄存器写入、协议底层、内部日志或低频诊断流程。保留影响正常操作的通信状态解释。

## 05 生产记录 / Records

### 5.1 记录页面 / Records Workspace
- 5.1.1 记录类型及筛选 / Record Types and Filters：日期、批次、不同记录页的用途。素材 UI-19。
- 5.1.2 批次及运行记录 / Batches and Runs：批次累积量、Run 记录、已记录的生产信息；查看不改变生产批次。素材 UI-20。

### 5.2 NG 胶囊与历史图像 / NG Capsules and Historical Images
- 5.2.1 缺陷回看与多次曝光 / Defect Review and Multiple Exposures：四张实际关联图像及上下视图；查找标签、面积和通道。素材 UI-21、IN-02。
- 5.2.2 Filtered 记录 / Filtered Records：正常四次曝光中最终 OK 且存在面积排除的记录，不能解释为所有 OK 或所有误检。素材 UI-22、IN-04。
- 5.2.3 周期记录与图像查看 / Cycle Records and Image Review：逐曝光记录、通道矩阵与实际可用的关联原图。素材 UI-23。

### 5.3 报告与追溯 / Reports and Traceability
- 5.3.1 生产报告 / Production Reports：选择批次、报告中的数量与合格率，报告覆盖范围。素材 RP-01。
- 5.3.2 缺陷分类报告 / Defect Classification Reports：一粒胶囊可有多个缺陷类别，分类数量不能简单相加当作 NG 粒数。素材 RP-02。
- 5.3.3 报警与操作记录 / Alarm and Audit Records：按常见权限与实际用途简述查询与导出；无需解释审计内部机制。素材 UI-24、RP-03（可选）。

### 5.4 利用记录分析检测效果 / Using Records to Evaluate Inspection
- 5.4.1 误检、漏检与合格率 / False Positives, False Negatives and Pass Rate：区别真实标签与软件输出；仅有 NG 记录不能估计漏检。素材 G-10、IN-06。
- 5.4.2 按缺陷类别分析 / Defect-based Analysis：查阅真实案例、图像质量、区域面积及筛选效果，提出下一步验证方向。
- 5.4.3 模型和配方变更的比较 / Comparing Models and Recipes：同一批准验证集和明确参数，不把不同批次未经控制的合格率直接当作模型性能比较；形成有用的训练难例。

## 06 相机与视觉系统 / Cameras

### 6.1 相机管理 / Camera Management
- 6.1.1 页面与相机身份 / Workspace and Camera Identity：配置通道数、固定相机/通道身份、设备识别与连接状态。素材 UI-25。
- 6.1.2 连接、断开及预览 / Connect, Disconnect and Preview：已保存设备映射、被动外部触发预览、生产停止后连接可保留；解释无图像不必然是断线。素材 UI-26。
- 6.1.3 图像输入与离线浏览 / Image Sources and Offline Review：临时导入、Folder Replay 的用途，真实输入与演示；不将离线回放作为实际 PLC 剔除验证。素材 UI-27。

### 6.2 通道区域配置 / Channel ROI Configuration
- 6.2.1 ROI 与图像坐标 / ROI and Image Coordinates：每台相机六个通道、区域归属、覆盖两个光学视图。素材 IN-01、G-11。
- 6.2.2 ROI 调整与核查 / ROI Adjustment and Verification：常用图形/数值操作，检查边界与通道对应、避免裁切；不规定未经验证的统一尺寸。素材 UI-28。
- 6.2.3 保存与重新应用 / Save and Reapply：设备级配置、保存和 Inspect 重新选择，实际图像尺寸检查。相机映射变更须在检测停止并断开连接的条件下按实际流程进行。

### 6.3 光学成像基础 / Optical Imaging Fundamentals
- 6.3.1 相机、镜头、光源与反射镜 / Camera, Lens, Illumination and Mirrors：组件作用及设备实际观察路径，使用无型号突出照片。素材 OP-01、OP-02、G-02。
- 6.3.2 曝光、增益与运动模糊 / Exposure, Gain and Motion Blur：通过实际对比解释亮度、噪声与运动清晰度。素材 OP-03、OP-04。
- 6.3.3 对焦、光圈与景深 / Focus, Aperture and Depth of Field：理解清晰度与光量的权衡，不提供未验证的设定值。素材 OP-05。
- 6.3.4 视野、分辨率与像素尺度 / Field of View, Resolution and Pixel Scale：缺陷在图像上的可见特征；像素与物理单位的关系需要标定。素材 OP-06、G-12。
- 6.3.5 光照、反光与颜色 / Lighting, Reflection and Color：不同照明与镜面反射对分割、颜色和小缺陷的影响；使用实际图像对照。素材 OP-07、IN-09。
- 6.3.6 相机参数工具 / Camera Configuration Utility：实际使用工具的常用观察和设置入口，与主软件功能区分；仅在实机证实后介绍具体操作。素材 OP-08。

### 6.4 日常维护与相机更换 / Routine Maintenance and Camera Replacement
- 6.4.1 图像质量日常检查 / Routine Image Quality Checks（讨论）：观察污染、模糊、过曝、阴影、遮挡、通道间一致性；建议建立基准图像，实际检查频次待定。素材 OP-09。
- 6.4.2 视觉组件维护 / Optical Component Care（讨论）：镜头、反射镜、光源外观检查及适用的清洁建议；不未经设备确认指定拆装方式、清洁剂或周期。
- 6.4.3 相机更换后的配置 / Configuration After Camera Replacement（讨论）：停止检测、确认安全维护条件、更换后的网络/参数恢复与设备映射、尺寸和 ROI 核对。机械安装、电气接线按设备相关人员流程；实际操作示例待确认。素材 UI-29、OP-08。
- 6.4.4 恢复生产前的视觉确认 / Visual Checks Before Returning to Production：图像一致性、参考样品、通道归属、检测效果与通信状态，按已确认设备流程恢复生产。

## 附录 / Appendices

附录以任务快速查阅为目标，不复写原理章节。A–E 为优先内容，F–G 按素材决定。

- A 日常生产快速指南 / Quick Production Guide：从登录、批次、配方选择到开始、停止、查看报告。引用第 2、5 章。
- B 新产品配方建立与验证 / New Product Recipe Setup and Validation：复制/新建、模型、标签、面积、长度/颜色、存档、保存/重新选择、参考样本验证。引用第 3 章。
- C 模型更新与标注准则检查 / Model Update and Annotation Checklist：确认缺陷定义、标签一致性、数据划分、自动标注复核、训练验证、导出、导入、复测。引用 3.2–3.5。
- D 相机维护与恢复检查 / Camera Maintenance and Recovery Checklist：日常图像、映射、相机更换后、ROI 及确认生产。引用第 6 章；细节随设备确认。
- E 常见现象与问题排查 / Troubleshooting by Symptom：无图像、单相机异常、固定通道 NG、误检、漏检、阈值设置、模型启动异常、通信异常、历史图像缺失；按成像→模型→规则→结果/通信组织，不要求尝试无依据的参数改动。
- F 参数与中英文术语 / Parameters and Bilingual Glossary：常见参数、单位、实际界面名、缩写和概念。可合并成一份简洁参考。
- G 代表性缺陷案例 / Representative Defect Cases（可选）：仅收录正文以外仍有独立教学价值的案例；原图、标注、Mask、判定、观察要点与相关小节。不要为案例数量重复相同内容。

## 推荐推进顺序

1. Phase 1：先核对当前版本的 Recipe 与 Records 相关功能，准备英文截图、真实分割和面积过滤案例，用于样稿。
2. Phase 2：优先制作 3.3.2 与 3.5.2–3.5.3 的组合样稿，覆盖原理图、真实软件截图、实际检测案例、双语及参数解释。若素材不足，先用可验证的独立示意图做版式测试，不冒充实物证据。
3. 样稿风格确定后，逐节完成 Recipe；Inspect、PLC、Records 与 Cameras 可独立推进，章节编写顺序无需与最终目录相同。标注准则和维护方法由对应小节集中讨论。
4. 正文稳定后生成附录，最后核对全篇中英文、流程、截图与图表交叉引用。

未决：当前部署版本、实际启用/常用功能、训练工具操作、光学结构和维护方式。现阶段目录不授权任何软件修改或现场操作。
