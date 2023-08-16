# 딥러닝을 활용한 이미지 기반 질의 응답 (VQA) 시스템

## 소개
- 주제 : 딥러닝을 활용한 이미지 기반 질의 응답 시스템
- 목표 : 활용가치가 높은 멀티모달 딥러닝 모델 제작
- 데이터 출처 : [데이콘](https://dacon.io/competitions/official/236118/data)

## 프로젝트 배경
- 배경
  - 이미지에서 원하는 정보를 얻기 위한 방법 중 사람이 육안으로 검수하는 일은 시간이 많이 걸리고 수고스러우며, 이러한 수고로움을 개선하고자 고안됨
- 기대효과
  - 이미지와 텍스트를 동시에 활용하는 멀티모달은 향후 발전 가능성이 높은 분야로 소방안전분야, 생활안전분야, 육아보조 등의 다양한 이미지 활용 분야에서 서비스가 가능함

## 데이터
- 데이터 소개
  - 이미지(.jpg), 텍스트(.csv) 데이터
  - 이미지 데이터 구성
    - 학습용 데이터 50,000여개 / 테스트용 데이터 10,000개
  - 텍스트 데이터 구성

    |변수|내용|
    |:------:|:---:|
    |ID|질문 ID|
    |image_id|이미지 ID|
    |question|이미지 관련 질문|
    |answer|질문에 대한 정답|
  - 데이터 예시

    <img width="611" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/98e49ed7-17aa-4cd7-befc-c2c9b56a6efa">

## EDA 
- 이미지 크기 확인

  <img width="608" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/b55dd95b-bb94-465f-9f2f-f436f56f0c68">
- 질문 형태 확인

  <img width="611" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/cc85b616-a44b-4e90-8aea-70c612d6e99a">
- 질문 단어 개수 확인

  <img width="608" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/4f221db2-4ca7-4540-8517-271320acc724">
- 답변 형태 비율 확인

  <img width="610" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/ae5425c2-628c-4647-8454-5606e4bd0483">

## 데이터 전처리
- text 데이터 내 동일 이미지 데이터 발견
- image_id를 기준으로 중복값 확인 후 50000개 추출
- 해당 image_id로 train_50000 이미지 폴더 생성

## 모델 선정
- ResNet50
  - 이미지 처리를 위한 CNN계열 신경망
  - 잔차연결을 통해 기존 네트워크보다 더 적은 파라미터로도 깊은 구조 생성 가능
  - 코랩 GPU 환경에서는 layer가 많을수록 한계가 있을 것이라 판단 50layer로 결정
- GPT2
  - 언어 처리를 위한 Transformer 기반의 언어 모델
  - 단방향 언어모델로 문장을 순차적으로 처리하기 때문에 구현과 학습이 상대적으로 간단
  - VQA에서는 질문의 의미를 더 잘 이해하고  ResNet과 결합해 더 자연스러운 답변을 생성할 것으로 기대됨

## 하이퍼 파라미터 조정
- batch_size : 8 ~ 64
- learning_rate : 1e-3 ~ 1e-5
- epoch : 1 ~ 5
- 구현 결과
  - batch 사이즈는 8, 학습률은 1e-4 일 때, loss 0.1726 / accuracy 0.2136 으로 나타났고, questino에 대한 answer을 잘 구현하지 못한다고 판단
  - 따라서, 답변이 yes/no인 경우의 데이터로 다시 구현 시도

## 데이터 전처리 2(재진행)
- yes/no 질의응답 데이터 셋 중 train 이미지 10000개 / test 이미지 2000개 로 조정

## 최종 모델 구현 결과
- accuracy 0.6435로 증가
- 데이터 셋 조정 후 batch 사이즈가 64인 경우 더 높은 accuracy 결과값 발견

<img width="609" alt="image" src="https://github.com/seolryungJ/DL_project/assets/131641261/443afd56-f996-46b7-a006-abfbcb329d74">

## 결과 및 개선점
- 결과
  - 원 데이터 셋보다 Yes/No로 구성한 데이터 셋의 정확도가 더 높게 도출됨
  - 정확도를 기준으로 최종 조정 모델을 epoch 10, batch 64, lr 1e-4인 모델로 선정함
  - 최종 모델의 정확도는 0.6435, loss 값은 0.0195로 나타남
- 개선점
  - Resnet + GPT2 말고도 다른 결합 모델로 사용하고 결과 값을 비교하여 더 나은 모델로 선정
  - 코랩으로 사용하여 진행하다보니 GPU 할당량이 부족하여 높은 epoch를 진행하지 못함
  - 층수를 50 layers 이상으로 늘려서 진행한 모델과 성능 비교 진행


