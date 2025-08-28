https://arxiv.org/pdf/2101.06085

---

# 1. Abstract

## Purpose

However, there is still a significant gap in performance between these real-time methods and the models based on [[dilation backbones]]. **To tackle this problem**, we proposed a family of efficient backbones specially designed for real-time semantic segmentation.

## Method

The proposed deep dual-resolution networks (DDRNets) are composed of two deep branches between which multiple [[bilateral fusions]] are performed.

Additionally, we design a new contextual information extractor named Deep Aggregation Pyramid Pooling Module (DA[[PPM]]) to enlarge effective receptive fields and fuse multi-scale context based on low-resolution feature maps.

## Result & Conclusion

Our method **achieves a new state-of-the-art trade-off between accuracy and speed** on both Cityscapes and CamVid dataset. In particular, on a single 2080Ti GPU, DDRNet-23-slim yields 77.4% mIoU at 102 FPS on Cityscapes test set and 74.7% mIoU at 230 FPS on CamVid test set. With widely used test augmentation, **our method is superior to most state-of-the-art models and requires much less computation.**

---

# 2. Introduction

From then on, dilated convolutions based backbones with context extraction modules have become the standard layout widely used in a variety of methods, including DeepLabV2 [9], DeepLabV3 [10], PSPNet [11], and DenseASPP [12].

Since semantic segmentation is a kind of [[dense prediction]] task, neural networks need to output high-resolution feature maps of large receptive fields to produce satisfactory results, which is computationally expensive.

**This problem is especially critical** for **scene parsing of autonomous driving** which requires enforcement on very large images to cover a wide field of view.

 •••

Recently, some competitive real-time methods aiming at semantic segmentation of road scenes are proposed. These methods can be divided into **two categories**. One utilizes the GPU-efficient backbones, especially ResNet-18 [21]–[23]. The other develops complex lightweight encoders trained from [^1]scratch, one of which, BiSeNetV2 [24] hits a new peak in terms of real-time performance, achieving 72.6% test mIoU at 156 FPS on Cityscapes.

•••

However, except for [23] using extra training data, **these recent works do not show the potential for higher quality results.**

•••

In this paper, we propose the **dual-resolution networks** with deep high-resolution representation for real-time semantic segmentation of high-resolution images, especially road-driving images.

Our DDRNets **start from one [^2]trunk** and then divide **into two parallel deep branches** with different resolutions.

**One deep branch** generates relatively **high-resolution feature maps** and **the other** extracts **rich semantic information** through multiple downsampling operations.

Multiple bilateral connections are bridged between two branches to achieve efficient **information fusion**.

Besides, we propose a novel module named **[[DAPPM]]** which inputs low-resolution feature maps, extracts multi-scale context information, and merges them in a [^3]cascaded way.

Before training on semantic segmentation dataset, **the dual-resolution networks are trained on ImageNet** following common paradigms.

According to extensive experimental results on three popular benchmarks (i.e., Cityscapes, CamVid, and COCOStuff), **DDRNets attain an excellent balance between segmentation accuracy and inference speed.** Our method **achieves new stateof-the-art accuracy on both Cityscapes and CamVid** compared with other real-time algorithms, without [[attention mechanism]] and extra [^4]bells or whistles.

•••

The main contributions are summarized as follows:
-  A family of novel bilateral networks with deep dualresolution branches and multiple bilateral fusions **is proposed for real-time semantic segmentation** as efficient backbones.
 - A novel module is designed to harvest rich context information by combining feature aggregation with pyramid pooling. When executed on low-resolution feature maps, it leads to little increase in inference time.
-  Our method achieves a new state-of-the-art trade-off between accuracy and speed with the 2080Ti, 77.4% mIoU at 102 FPS on Cityscapes test set and 74.7% mIoU at 230 FPS on CamVid test set. To our best knowledge, we are the **first to achieve 80.4% mIoU in nearly real time (22 FPS) on Cityscapes** only using [^5]fine annotations.

---

# 3. Related Work



---

# 4. Conclusion

we are the first to introduce deep highresolution representation into real-time semantic segmentation and our simple strategy outperforms all previous real-time models on three popular benchmarks.

---

# Foot notes

[^1]: 전이 학습의 반대 개념으로, 아무런 사전 학습 없이 처음부터 학습시키는 것.

[^2]: 사전적 의미는 the main stem of a tree apart from limbs and roots. 여기서는 다음을 의미한다. main initial part of the neural network architecture before it splits into multiple branches.

[^3]: = sequential way

[^4]: items or features that are useful or decorative but not necessary. (멋으로 덧붙이는 부가 기능.)

[^5]: 정교한 어노테이션(좋은 데이터 라벨링). 거친 어노테이션(Coarse Annotations)과 반대되는 개념.
