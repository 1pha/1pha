# DINO: Emerging Properties in Self-Supervised Vision Transformers
* [Paper Link](https://arxiv.org/abs/2104.14294)
* [GitHub](https://github.com/facebookresearch/dino)
!!! abstract "TL;DR"
    * Aims to impact of SSL on ViT, but not from convnets
    * SSL ViT Features contain **explicit information about the semantic segmentation of an image**
    * These features are also excellent k-NN classifiers (78.3% top-1 ImageNet)
    * Linear Evaluation 80.1% top-1 ImageNet
    
## Introduction
* ViT is competitive with convnets, but requires more training data and features do not exhibit unique properties
* 언어 모델에서 거둔 Transformer의 성공이 CV에서는 왜 일어나지 않을까? 에서 착안한 논문
    - Transformer라는 구조 자체도 유의미하지만, self-supervised learning을 transformer architecture 위에 한 것에서 큰 성공을 얻은 것이 아닌가라는 질문에서 시작
* 그동안 mode collapse를 피하거나 하는 등의 방법으로 SSL을 