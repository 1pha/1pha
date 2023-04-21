# Explainable AI TL;DR

## Transformer Explainability

| Paper | TL;DR | Notes | Source | Date |
|--|---|---|---|---|
| [AttCAT](https://openreview.net/forum?id=cA8Zor8wFr5)  | Attention weight뿐 아니라 feature value의 magnitude + residual connection이 고려한 방법을 제시하였다.| 크게 novelty가 있다고 여겨지지는 않는다 (그럼 너가해보던가). 전체 attention을 곱하는 형식인 Rollout이 어지간하면 0으로 수렴하는 현상을 여기서도 포착할 수 있었다. Reference로 달아둔 논문들은 Transformer explainability에서 그 역할이 중요하기 때문에 꼭 정리해놓자. Rating: 4658 | NeurIPS(2022) | 2022 |



## Visual Counterfactual Explanations
| Paper | TL;DR | Notes | Source | Date | Code |
|--|---|---|---|---|---|
| [DVCEs](https://openreview.net/forum?id=7SEi-ISNni7) | Classifier Guidance 방식에서 non-robust classifier를 이용하였다. 타겟으로 하는 라벨이미지와 현재 timestep에서 원본이미지를 denoise한 이미지와의 거리를 최소화하는 방향으로 Visual Counterfactual을 만들어냈다. 또한 cone-regularization으로 non-robust classifier가 가지는 noisy gradient 문제를 잡았다. |  Rating: 5677 | NeurIPS(2022) | 2022 | [Link](https://github.com/valentyn1boreiko/DVCEs) |