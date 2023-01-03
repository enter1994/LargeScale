# LargeScale 대규모 데이터 학습
## 분산 학습 및 HALP(High Accuracy Low Precision)를 적용한 대규모 데이터 학습
- 인공지능융합학과 김가연, 심유라, 컴퓨터공학과 박도영

## Senario
- 딥러닝 모델을 학습하는 과정에서, 대규모 데이터셋을 학습시킬 경우 많은 연산 시간이 소요
  - 학습 시간을 단축하는 기술인 **분산 학습** 적용
- 'Flickr8k' 데이터를 이용한 이미지 캡셔닝
![image](https://user-images.githubusercontent.com/45381907/200161064-b25c16cd-7b6a-471e-b4e6-c406a3e848dc.png)
  - 출처 : https://medium.com/@raman.shinde15/image-captioning-with-flickr8k-dataset-bleu-4bcba0b52926
- 모델
  - image feature extract model : **ResNet**
  - captioning model : **Transformer**
  
  
### 프로젝트 목표

---

- 대규모 데이터의 효율적으로 학습
- Google Cloud Platform(GCP)에 여러 서버에 나눠져 있는 GPU를 이용한 데이터 분산 학습
- 서버 통신 간 높은 비용이 발생하는데 이를 줄이기 위한 HALP 논문 적용

![image](https://user-images.githubusercontent.com/80090973/210303224-842df850-fd3f-4441-b1e6-fcaf08cb0e14.png)

### 기여한 내용

- GCP서버에 학습을 위한 Ubuntu환경 구축하였습니다. (총 3개의 서버, 4개의 GPU)
- GCP 내에서 다른 서버 간 통신을 위한 코드를 구현하였습니다.
    - nn.torch.DataParallel
- 데이터 학습을 위한 모델 코드 구현하였습니다.

### Dataset

---

- Flickr8k

![image](https://user-images.githubusercontent.com/80090973/210303248-422681ed-ff8d-444f-99dc-99b2c29e5464.png)

### 프로젝트 내용

---

- 대규모 데이터를 딥러닝 모델을 이용하여 학습하기 위해서는 오랜 시간과 비용이 소모
- 이를 해결하기 위해 여러 GPU를 사용하여 학습 진행
- 학습 진행과정에서 Master 서버와 Worker 서버가 통신하면서 발생하는 서버 통신 비용이 큼 → 분산 학습의 효과가 줄어듬
- 서버 통신과정에서 학습시 주고받는 Gradient 크기를 압축
    - High-Accuracy Low-Precision Training (HALP) 논문 구현

### 프로젝트 결과

---

![image](https://user-images.githubusercontent.com/80090973/210303281-3f3a291c-e7a1-412d-8e53-3ab7a608e581.png)


- HALP를 적용하지 않고 GPU를 늘리며 학습을 진행했을 때 loss가 조금씩 내려가는 것을 확인
- 하지만 서버간 통신 비용이 컸기 때문에 학습 에폭당 학습 시간은 크게 변화가 없는 것을 확인
- HALP를 적용하여 Communication Cost를 줄이고자 하였지만 HALP 논문에서 권장한 배치 사이즈를 GPU 메모리의 부족으로 인해 적용할 수 없었음
- HALP에서 사용한 모델이 아닌 Transformer 모델을 사용하면서 오히려 학습 시간이 대폭 증가하는 결과를 확인
- 간단한 모델일 경우 적절한 hyperparameter로 설정한 HALP를 적용하여 Data parallelism을 진행한다면 학습 시간이 linear speed-up 형태를 띄울 것이라 기대
