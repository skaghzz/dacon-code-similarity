# 데이콘 코드 유사성 판단 AI 경진대회

[데이콘 코드 유사성 판단 AI 경진대회](https://dacon.io/competitions/official/235900/codeshare)

기간: 22년 5월 15일 ~ 22년 6월 10일

## 아이디어

### 데이터 생성에서 사용된 아이디어

1. contrastive learning의 개념을 적용하여 negative sample을 선정하였습니다.
2. 학습할 모델보다 성능이 좋은 다른 모델(UniXcoder)을 이용해서 dataset을 sampling을 하였습니다. 이는 학습모델(GraphCodeBert)이 생각하지 못한 경우를 다른 모델로부터 배울 수 있다는 가정에서 접근하였습니다.
3. 전처리는 간단하게 주석만 제거했습니다.

### 학습에 사용된 아이디어

1. [GraphCodeBert](https://github.com/microsoft/CodeBERT) - pretrained model 사용
2. [Label smoothing](https://arxiv.org/pdf/1906.02629.pdf)
3. tokenize시 truncation 방향을 right에서 left로 변경

## 결과

- public: 0.96134
- private: 0.96139

## 시행착오s

1. GraphCodeBert Clone detection에서 사용된 모델 그대로 사용 시 accuracy 0.77 나왔습니다.
    - model head를 linear layer(RobertaForSequenceClassification)로 변경한 경우 성능이 급격히 상승함
    - 제공되는 코드는 data flow를 모델 input으로 이용하는데, 컴퍼티션에서 제공되는 코드는 길이가 길어서 data flow를 통해 보존하려던 소스코드의 특징을 오히려 더 날려버리게 되는 상황이 연출된 것 같음.
    - GraphCodeBert의 Clone detection은 java function단위로 비교하지만, 컴퍼티션은 function보단 큰 단위의 알고리즘 문제코드임.
2. 아주 긴 학습 시간
    - dataset을 늘리면 그만큼 학습 시간이 정말 길다.
    - 컴퍼티션에서는 시간을 고려해야한다.
    - 1 epoch에 12시간정도 소요되었다.

## 추가로 할만한 방법

1. k-fold training
2. ensemble
2. model head 변경
3. 기타 hyperparamter 변경 방법 시도(layer wise learning rate 조절 등)
4. Contrastive learning - [SimCSE](https://arxiv.org/abs/2104.08821)