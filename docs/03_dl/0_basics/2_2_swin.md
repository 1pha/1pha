# Swin

# Swin_v2
!!! abstract "TL;DR"
    * Large-scale ViT in computer vision
        - Residual-post-norm + cosine attention
        - Log-spaced continuous position bias: 작은 이미지 pre-train -> 큰 이미지에 fine-tune할 때 생길 수 있는 relative positional bias의 값 크기 차이로 인한 성능 저하를 막기 위해 positional bias에 log만 추가했음
        - 학습할 때 SSL 방법을 이용했음
    * 여러 실험을 통해 위에 대한 근거들을 정리

## Introduction
1. Instability Issue
    * 모델의 규모가 커질수록 후반 레이어의 activation값이 너무 커지게 된다 (Figure 2)
    * ![Figure 2](/docs/assets/swinv2_fig2.png)
    * **Suggestion**
        - Residual post-normalization
        - Scaled Cosine Attention

2. Image Resolution Issue
    * Pre-trained 모델이 학습했던 resolution과 다른 resolution을 넣어서 학습하면 성능이 크게 저하되는 점
    * 이는 Relative positional embedding이 너무 값이 크게 들어가기 때문에 resolution이 오르면 이 embedding값이 vector embedding에 많은 영향이 있기 때문으로 판단
    * **Suggestion**
        - Log-spaced continuous position bias
