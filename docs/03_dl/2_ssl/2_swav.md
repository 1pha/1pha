# SwAV: Unsupervised Learning of Visual Features by Contrasting Cluster Assignments
* [Paper Link](https://arxiv.org/abs/2006.09882)
* [GitHub](https://github.com/facebookresearch/swav)
!!! abstract "TL;DR"
    * Deep Cluster의 저자로 Self-supervised learning의 proxy task를 cluster 기반으로 제안했다.
        * 여기서 서로 다른 augmented image의 feature vector를 상대의 prototype과 가까워지도록 맞춰주는 loss를 제안한다.
    * 논문에서 `multi-crop`이라는 data augmentation strategy를 제시하였다.