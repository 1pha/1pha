# Brief History of Diffusion-based Generative models and their variants

* **BigGAN**: 2019. 2 (ICLR 2019) | DeepMind
* **Generative Modeling by Estimating Gradients of the Data Distribution**: 2019. 7 | Stanford
* **DF-GAN**: 2020. 8
    - Text-to-Image
* 📌 **DDPM**: 2020. 12 (NeurIPS 2020) | Berkeley → Google Research
* **DDIM**: 2020. 10 | Stanford
    - 기존 DDPM이 Noising process가 Markovian이라 바로 이전의 timestep noised image에만 의존하여 전체 프로세스를 만들어내는데 시간이 많이 걸린다. 여기서 Non-markovian으로 $x_0$에서 $x_T$로 가는 과정을 deterministic하게 forward process를 일부만 sampling하여 train하고 추론할 때도 마찬가지로 전체 step을 다 거치지 않아 accelerated inference가 가능하다.
* **Score-based Generative Modeling through Stochastic Differential Equations**: 2020. 11 | Google Research
* **VQGAN**: 2020. 12 (CVPR 2021) | CompVis
    - ConvNet과 Transformer가 generation task에서 각각 잘하는 부분을 다 사용해서 image tokenization을 진행
        - ConvNet은 inductive bias로 인해 local 정보를 많이 가지고 있다. 하지만 global 정보를 가지고 있지 않다.
        - Transformer model은 low inductive bias 덕분에 expressive하다 (워딩 때문에 혼란스러운데 아마 global information을 보유한다 정도로 이해하면 되지 않을까 싶다). 하지만 고화질의 이미지를 만들기는 어렵다.
    - ConvNet으로 image feature map을 만든 후 이를 quantize하여 latent image code를 만든다. 또한 이 code를 decoder에 태워 GAN 구조로 학습을 시킨다.
    - 그 후 image를 tokenize하여 sequence로 보유한 후 이를 language model처럼 학습시킨다 (Auto-regressive하게 next token prediction 가까움)
    - Auto-regressive한 특성을 활용하여 conditioning을 할 수 있다: 시작점에 conditioning information을 prepend해서 추론

* **XMC-GAN**: 2021. 1 (CVPR 2021) | Google Research
    - Text-to-Image


* 📌 **Improved DDPM**: 2021. 2 | OpenAI
    - DDPM의 여러 성질들을 이해하는데 중요한 논문. 개인적으로 어떤 걸 탐구하기 위해 이게 왜 잘되었는지 실험해보려면 이렇게 해야하지 않나라는 생각.
        - DDPM이 log-likelihood 면에서 다른 Generative model 보다 밀리는 이유
            - log-likelihood 실험하는 중 conditional-GAN과 비교해야해서 timestep embedding에 class-embedding을 같이 녹여넣었는데 이것이 conditional-diffusion의 시작이 아닌가 라는 생각도 든다
        - Noise scheduling을 linear에서 cosine으로 바꾸자!: linear는 후반 step에서 너무 이미지랑 관련 없는 noise-to-noise recon 밖에 하지 않는다.
            - VLB를 prior / posterior의 interpolate된 것을 L_simple에서 같이 쓰면 잘나온다 (하지만 이또한 보정이 필요했음)
            - 이 과정에서 interpolation이 rescaling으로 인해 shorter diffusion을 가능하게 한다... 즉 더 적은 sampling step을 쓸 수 있다는데 정확히 이해는 가지 않는다.
        - Sampling step T가 클수록 잘나온다(사실 어느정도 예상했음). 그런데 T가 늘면 sampling이 아주 느리다 → 빨리 뽑는 법을 떠올려보았다.
        - Model scaling 관점에서도 고민을 했음: 앞으로 GPU는 빵빵해질 것이므로 더 큰 모델도 테스트해보았다...ㅋㅋ;
    
* **DallE**: 2021. 2 | OpenAI
    - Discrete variational autoencoder (DVAE)로 이미지를 $32^2$ grid로 만들어줌. 이 때 codebook은 총 8192개의 고유 token이 있음. 이를 256 BPE-encoded text token으로 text query를 toeknize하고 방금 만든 $32^2$ 이미지의 image token들과 concatenate, autregressive하게 학습
    - 얘도 Likelihood $\ln p_{\theta,\psi}(x, y)$를 학습하는 것으로 Lower bound를 학습한다.
* 📌 **CLIP**: 2021. 3 | OpenAI
    - Generative modeling이라기 보다는 Image-text pair 학습 방법론으로 이렇게 학습된 encoder들을 활용한다.
* **SR3**: 2021. 4 | Google Research
* 📌 **Guided Diffusion (Classifier Guidance, ADM: Ablated Diffusion Model)** 2021. 5 | OpenAI
    - Pre-trained classifier의 gradient를 condition으로 활용하는 방법 제시
    - 그 밖에도 모델 구조 개선에 대한 고찰 진행
        - Increased depth: 성능은 높이지만 시간이 너무 오래 걸림 → discard
    - 사실 GAN에서 Classifier guidance와 비슷한 효과를 주는 것은 truncation, temperature scaling이다. 같은 걸 diffusion에도 적용해보았으나 잘되지 않았다: blurry/smooth images, low precision & recall
        - sampling할 때 temperature만큼 mean은 scaling, deviation은 나눠주기

* **Cascaded Diffusion (CDM)** 2021. 6 | Google Research
    - High-resolution fidelity를 위해 SR을 단계별로 진행
    - 
* **ImageBART**: 2021. 8 (NIPS 2021) | CompVis
* **Vit-VQGAN**: 2021. 9 | Google Research
* **LSGM**: 2021. 10
* 📌 **Classifier Free Guidance**: 2021 NeurIPS workshop | Google Research
    - Classifier guidance는 Sampling 조절에 엄청난 영향을 끼쳤으나 Pre-trained classifier의 gradient를 요구하기 때문에 굉장히 computational burden이 높다.
    - 그 하나의 모델이 condition까지 같이 받아서 guidance를 생성하는데, 이 때 예측하려는 noise image = conditional + guidance_scale * (conditional - unconditional)
        - 이는 p(c|z_t)가 Bayes rule에 의해 p(z_t | c) / p(z_t)에 비례하다는 점을 log를 씌워 유도한 것으로 이해하면 된다. 즉 이 이미지를 학습시킨다는 것은 p(c|z_t)가 가능하도록 생성하는건데 t-step의 noise가 낀 latent vector로부터 class를 예측하는 것을 뜻한다.
    - ? 전부터 궁금했는데 여기서 이 떄까지의 guidance는 text prompt가 아니라 일반적인 ImageNet Class 1000개를 임베딩해서 넣어놓는 형식인가? 이렇기 때문에 바로 다음에 나오는 GLIDE가 text-prompt를 CLIP으로 만들었다는 것에 대해 요지를 두는 것 같음

---

* **GLIDE**: 2021. 12 | OpenAI
    - Diffusion + Text Prompt Guidance의 시초라고 볼 수 있음 (text-to-image synthesis를 처음 한 게 아니라 diffusion에)
        - 이 때 text encoder embedding을 CLIP Guidance vs. Classifier-free guidance 두 가지 다른 방법을 사용하여 모델에게 제공해 이미지를 생성했다. 두 가지 방법으로 나온 이미지를 사람에게 보여줘서(+automated evaluation) 누가 더 진짜 같은지 비교했을 때 CFG가 이겼다. 참고로 여기서 CLIP Guidance는 classifier-guidance인데, classifier가 CLIPI text-encoder인 경우를 뜻한다.
        - CFG가 photorealistic + wide breadth of world knowledge를 가진다고 판단. DallE랑 비교했을 때 GLIDE가 더 photorealistic하다고 85%가 답변, caption과 align되었다고 69%가 답변
    - 일반적인 Diffusion model을 Text-prompt를 같이 넣어서 학습한 방식

* **MaskGIT**: 2022. 2 | Google Research
    - Auto-regressive 기반의 이미지생성 모델은 너무 오래 걸린다. 그리고 AR 방식 자체가 사람이 그림 그릴 때의 상황과 비교해보면 말이 되지 않는다 (사람은 그림그릴때 왼쪽 위부터 오른쪽 아래를 차례로 채우는 것이 아니라 여기 그렸다가 저기 그렸다가 하기 때문)
    - 이 두 가지 motivation으로부터 Parallel decoding한 방법을 제시한다. 기존 VQGAN처럼 quantized image token -> recon하는 task와 동시에 MLM의 방식과 비슷한 MVTM(Masked Visual Token Modeling)를 수행하여 decoding stage 또한 학습한다. 이는 bidirectional하게 token이 다른 모든 token들을 참조하여 이미지를 만들어낼 수 있다. (AR 방식의 경우 앞에서부터 차근차근 decode하다보니 뒤를 참조하지못함). 즉 이미지가 동시에 masking과 class token 서로를 참조하며 혼돈의 카오스로부터 만들어지는 이미지랄까. 기존의 32*32=1024 토큰맵을 예측하는 task라면 말그대로 1,024 step을 밟아야 이미지생성이 가능했다면, MaskGIT은 8 ~ 12step 안에 생성을 마칠 수 있다.
    - 접근도 신선했고 ablation도 잘된편이라 좋은 논문이라고 생각한다.

* 📌 **Latent Diffusion (LDM, or a.k.a. Stable Diffusion)** 2022.4 | CompVis
    - Pixel space에서 바로 예측하면 시간이 너무 오래 걸리기 때문에 latent space에서 예측할 수 있도록 연구. 확실히 시간적인 측면에서 단축이 많이 이뤄졌음.
    - Latent representation에 DM을 거는 것에 대한 고찰을 했음
        - 기본적으로 Generative modeling은 perceptual / semantic한 요소를 차례로 학습하는데, latent representation는 high-frequency detail을 없애면서 이를 학습(?) 이게 정확히 뭐지
    - Super resolution, Inpainting task

* **DallE 2(unCLIP)**: 2022. 4 | OpenAI
    - CLIP Objective를 통해 주어진 text query & image query에 맞춰서 image / text encoder를 학습하고, generation process에서는 CLIP Text embedding을 통해 prior를 만들어준 후 diffusion decoder에 넣어주는 형태로 generation을 진행한다.
    - CLIP Encoder + Diffusion Decoder 구조
        - 일반적으로 deterministic한 decoder와 다르게 non-deterministic하게 돌아간다 (stochastic term이 있는 것 같다). 이 non-deterministic한 성질이 deterministic한 GAN들이 비슷한 이미지를 내뱉는 것과 다르게 굉장히 다양한 이미지를 만들어낼 수 있다고 한다.
    - Encoder: Image embedding with text prompt -> 임베딩을 내놓으면 decoder한테 text prompt랑 같이 넘겨서 generate하는 구조
        - 여기서 encoder가 image embedding을 하는 과정이 두 가지로 나뉜다: AR vs. Diffusion
            - AR: Text-encoding + Text Embedding + Time Embedding + Image Embedding + Learned Queries를 시작점으로 Auto-regressive하게 z_i를 추론하고 l2로 학습
            - Diffusion: 얻어낸 Image embedding을 LDM 형식으로 학습 - image embedding을 normal로 만드는 forward process 그리고 원복하는 형태로 만들어주고 여기에 conditioning을 하면서 이미지가 

* **Imagen**: 2022. 5 | Google Research
    - LM의 역할이 매우 중요했다! **Frozen Pre-trained LLM으로 튜닝한 첫 사례**로 봐야할듯
    - Classifier guidance의 weight를 **dynamic thresholding**을 적용
        - Noise prediction한 이미지의 absolute value로부터 percentile을 구해서 그 값을 활용: 이게 어떻게 유의미한지 잘모르겠다.
    - **Efficient U-Net** 구조: 제일 큰 건 self-attention이 사라진 것 같다. text와의 cross-attention은 남겨두었다.
    - **DrawBench Dataset** 공개

* **Parti**: 2022. 6 | Google Research
    - Pathways AutoRegresstive Text-to-Image Models의 줄임말로 seq2seq 형태로 문제를 푼다. 보통 NMT에서 한 언어로부터 다른 언어로 번역하는 형식이라면, text-prompt를 source sequence, image token을 target sequence로 놓고 풀었다는 점.
    - Encoder-decoder style: Decoder-only 형태 (보통 text-image concat하고 하나씩 AR로 뽑아내는 구조)에서 변화를 가했다. text는 encode, image는 encoded text를 KV로 받아 AR형태로 뽑아내는 구조 (딱 seq2seq)
    - PartiPrompt: 1600개의 prompt로 다양한 카테고리와 모델의 물체 이해력을 평가할 수 있는 프롬프트가 포함되어 있다.
* **EDM (Denoise Score Matching)**: 2022. 10 (NeurIPS 2022) | NVIDIA
* **Variational Diffusion Models**: 2022. 12 | Google Research
* **MUSE**: 2023. 1 | Google Research
    - VQGAN의 discrete token으로 조금 더 efficient하게 뽑아낼 수 있다.
    - Parallel decoding 또한 활용했다는데 코드가 없어서 잘 모르겠다. DETR 말고는 내가 아는 사례가 없어서 알기가 어렵다.
* **ControlNet**: 2023. 2 | Stanford

* **Consistency Models**: 2023. 3 | OpenAI

# Generative Metrics
### Inception Score
* Multiplication of **Sharpness (S)** and **Diversity (D)**, obtained by Inception model. This is somewhat correlated to human evaluation but may not cover the whole thing.

| **Inception Score (IS)** was proposed by Salimans et al. [54], and it measures how well a model captures the full ImageNet class distribution while still producing individual samples that are convincing examples of a single class. One drawback of this metric is that it does not reward covering the whole distribution or capturing diversity within a class, and models which memorize a small subset of the full dataset will still have high IS [3].

### Frechet Inception Distance (FID)
* Caclulates **feature space distance** with Frechet Distance. FD calculates two normal distributions via sum of mean difference square and std difference square. This does NOT take how synthetic images compare to real images into account.
* [Reference: How to Evaluate GANs using FID](https://wandb.ai/ayush-thakur/gan-evaluation/reports/How-to-Evaluate-GANs-using-Frechet-Inception-Distance-FID---Vmlldzo0MTAxOTI)

| To better capture diversity than IS, **Fréchet Inception Distance (FID)** was propose judgement than Inception Score. FID provides a symmetric measure of the distance between two image distributions in the Inception-V3 [62] latent space. Recently, sFID was proposed by Nash et al. [42] as a version of FID that uses spatial features rather than the standard pooled features. They find that this metric better captures spatial relationships, rewarding image distributions with coherent high-level structure

### Precision and Recall | 2019.4 NVIDIA
| Finally, Kynkäänniemi et al. [32] proposed **Improved Precision and Recall metrics** to separately measure sample fidelity as the fraction of model samples which fall into the _data manifold_ (precision), and diversity as the fraction of data samples which fall into the sample manifold (recall).

### CLIP Score

### CAS: Classification Accuracy Score
Train ResNet-50 classifier soley on samples generated by the candidate model, and then measuring the classifier's classification accuracy on the ImageNet validation set.


# Generative Loss
* Perceptual Loss


# Research Groups
* CompVis Group
    - Computer Vision and Learning research group at Ludwig Maximilian University of Munich (formerly Computer Vision Group at Heidelberg University)
    - Robin Rombach, Andreas Blattmann, Dominik Lorenz, Patric Esser, Bjorn Ommer

* Google Research
    - Jonathan Ho
    - Tim Salimans
    - Christian Saharia
    - Ben Poole
    - Diederiek P. Kingma
    - Jarred Barber
    - Yang Song
    - Abhishek Kumar
    - Jascha Sohl-Dickstein
    - Diederik P. Kingma

* OpenAI
    - Prafulla Dhariwal
    - Radford Alec
    - Jong Wook Kim
    - Chris Hallacy
    - Gabriel Goh
    - Aditya Ramesh
    - Alex Nichol

* NVIDIA
    - Karras Tero
    - Miika Aittala
    - Timo Aila
    - Samuli Laine

* Stanford
    - Jiaming Song
    - Chenlin Meng
    - Stefano Ermon