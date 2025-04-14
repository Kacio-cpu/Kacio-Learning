## **摘要**
  Self-supervised feature reconstruction methods have shown promising advances in industrial image anomaly detection and localization. Despite this progress, these methods still face challenges in synthesizing realistic and diverse anomaly samples, as well as addressing the feature redundancy and pre-training bias of pre-trained feature. In this work, we introduce RealNet, a feature reconstruction network with realistic synthetic anomaly and adaptive feature selection. It is incorporated with three key innovations: First, we propose Strength-controllable Diffusion Anomaly Synthesis (SDAS), a diffusion process-based synthesis strategy capable of generating samples with varying anomaly strengths that mimic the distribution of real anomalous samples. Second, we develop Anomaly-aware Features Selection (AFS), a method for selecting representative and discriminative pre-trained feature subsets to improve anomaly detection performance while controlling computational costs. Third, we introduce Reconstruction Residuals Selection (RRS), a strategy that adaptively selects discriminative residuals for comprehensive identification of anomalous regions across multiple levels of granularity. We assess RealNet on four benchmark datasets, and our results demonstrate significant improvements in both Image AUROC and Pixel AUROC compared to the current state-ofthe-art methods.
  【译】自监督特征重建方法在工业图像异常检测与定位领域展现出显著进展。尽管如此，这些方法在生成逼真多样的异常样本、解决预训练特征冗余及预训练偏差方面仍存在挑战。本研究提出RealNet——一种集成真实合成异常与自适应特征选择的特征重建网络，其包含三项关键创新：首先，我们提出强度可控扩散异常合成（SDAS），这是一种基于扩散过程的合成策略，能生成符合真实异常样本分布且具有多级异常强度的样本；其次，开发异常感知特征选择（AFS），该方法通过筛选具有代表性和判别力的预训练特征子集，在控制计算成本的同时提升检测性能；最后，引入重建残差选择（RRS），该策略自适应选择判别性残差，实现多粒度异常区域的全面识别。我们在四个基准数据集上评估RealNet，结果表明其图像AUROC与像素AUROC指标均显著优于当前最先进方法。
## **背景与现有方法的问题**
  在工业图像异常检测（Anomaly Detection and Localization）中，自监督特征重建方法（Self-supervised Feature Reconstruction）通过重构正常样本的特征来检测异常区域（异常会导致重构误差增大）。然而，现有方法存在以下主要问题：
  1.异常样本合成不够真实和多样：传统方法（如CutPaste、Perlin噪声、GAN生成）合成的异常样本往往过于简单或分布偏离真实异常，导致模型泛化能力不足。例如，CutPaste通过裁剪粘贴生成局部异常，但难以模拟复杂纹理或渐变异常。<br>
  2.预训练特征冗余与偏差问题：大多数方法直接使用预训练模型（如ResNet、ViT）提取的特征，但这些特征可能包含大量与异常无关的信息（冗余），甚至因预训练任务（如ImageNet分类）引入偏差。例如，某些通道可能对异常不敏感，反而增加计算负担。
  3.多粒度异常定位不充分：
现有方法通常仅使用单一层级的重构残差（如最后一层特征），难以覆盖不同尺度的异常（如微小缺陷与大面积损坏）。
## **该论文创新点**
RealNet通过以下三个关键技术解决了上述问题：<br>
(1) Strength-controllable Diffusion Anomaly Synthesis (SDAS)
创新点：利用扩散模型（Diffusion Model）生成逼真且强度可控的异常样本。<br>
通过调节扩散过程的噪声强度，生成从轻微到严重的异常样本，更贴近真实工业缺陷的分布。<br>
相比传统方法（如GAN或噪声注入），扩散模型能生成更复杂的纹理和结构异常。<br>
改进：此前的方法（如DAAD）尝试用扩散模型生成异常，但未显式控制异常强度；SDAS通过参数化噪声强度，实现了更灵活的异常模拟。<br>
(2) Anomaly-aware Features Selection (AFS)
创新点：提出异常感知特征选择，从预训练模型中筛选对异常敏感的通道，去除冗余特征。<br>
通过评估特征通道在正常/异常样本上的响应差异（如KL散度），选择最具判别力的子集。<br>
例如，某些通道可能对表面划痕敏感，而对颜色变化不敏感。<br>
改进：传统方法（如PaDiM、PatchCore）直接使用所有预训练特征，导致计算成本高且引入噪声；AFS通过特征选择提升了效率与精度。<br>
(3) Reconstruction Residuals Selection (RRS)
创新点：自适应多粒度残差选择，结合不同层级的重构误差（如浅层细节与深层语义）定位异常。<br>
通过加权融合多层级残差（如ResNet的block1/2/3），避免单一层级遗漏某些异常类型。<br>
例如，浅层特征适合检测纹理缺陷，深层特征适合识别结构异常。<br>
改进：传统方法（如STFPM）仅使用固定层级的特征，而RRS动态选择最相关的残差，提升定位全面性。<br>
## **实验效果与对比**
RealNet在MVTec、BTAD等4个基准数据集上取得SOTA性能（Image AUROC/Pixel AUROC显著提升），例如：
优于基于GAN/VAE的异常生成方法（如AnoGAN、f-AnoGAN），因其生成样本不够多样。<br>
超越特征重建方法（如RD4AD、SimpleNet），因其未解决特征冗余问题。<br>
比多层级方法（如CFLOW、DiAD）更高效，因AFS和RRS减少了冗余计算。<br>
RealNet的核心贡献在于：
## **总结**
逼真异常合成（SDAS）→ 解决传统合成方法偏离真实分布的问题。<br>
特征选择（AFS） → 缓解预训练特征冗余与偏差。<br>
多粒度残差融合（RRS） → 提升异常定位的全面性。<br>
这些创新使RealNet在工业检测中更接近实际应用需求，尤其在复杂缺陷检测和计算效率之间取得了平衡。
