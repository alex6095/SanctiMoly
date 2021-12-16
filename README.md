# SanctiMoly
Neural network models for predicting the date / topic of news

## Dataset
[국립국어원: 모두의 말뭉치](https://corpus.korean.go.kr/) 사이트에서 신문 말뭉치 자료를 사용했다.

[Korpora가 제공하는 라이브러리](https://ko-nlp.github.io/Korpora/)를 활용하여 데이터를 불러왔다.

## Models
일부 모델의 경우 /runs 폴더 내에 Tensorboard Log가 존재한다.  
모델의 체크포인트의 용량이 큰 관계로, 공유된 링크를 통해 따로 다운받아야 한다.  
체크포인트는 주어진 링크에서 받아 주어진 경로에 넣는다.

### ToTheMoon (Date)
* 'smallnews' 셋으로 학습
* 'monologg/distilkobert' 기반 (Parameters frozen)
* Bert 뒤에 Classifier를 추가. 결과가 날짜를 의미하는 0 ~ 120 사이의 1차원 실수 값으로 나오도록 학습했다.
* Accuracy = 0.00994 @ Epochs 3

### BTTtoMars (Date)
* 'smallnews' 셋으로 학습
* 'monologg/distilkobert' 기반
* Bert 뒤에 Classifier 추가. 결과가 날짜를 의미하는 120차원의 원핫 벡터가 나오도록 학습했다.
* Accuracy = 0.3646 @ Epochs 20
* 체크포인트: /nBTT_20_ckpts/[Checkpoint files](https://drive.google.com/drive/folders/1nTLRIxLlKlmalRsgst69UkH0peiu1xxm?usp=sharing)
* 
### BTTtoMars_Large (Date)
* 'internetnews' 셋으로 학습
* 'monologg/distilkobert' 기반
* Bert 뒤에 Classifier 추가. 결과가 날짜를 의미하는 120차원의 원핫 벡터가 나오도록 학습했다.
* Accuracy = 0.2613 @ Epochs 23
* 체크포인트: /nBTT_20_ckpts/[Checkpoint files](https://drive.google.com/drive/folders/1nTLRIxLlKlmalRsgst69UkH0peiu1xxm?usp=sharing)

### BARDE_Alpha (Date)
* 'smallnews' 셋으로 학습
* 'gogamza/kobart-summarization' 기반 (인코더와 임베딩 parameters frozen)
* 결과가 날짜를 의미하는 문자열을 생성하도록 학습했다.
* Accuracy = 0.2899 @ Epochs 14
* 체크포인트: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)
  (폴더 구조도 유지해야 함)

### BARDE_Beta (Date)
* 'internetnews' 셋으로 학습
* 모델 구조는 BARDE_Alpha랑 같다.
* Accuracy = 0.3532 @ Epochs 10
* 체크포인트: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)
  (폴더 구조도 유지해야 함)

### FalconNine (Topic)
* 'internetnews' 셋으로 학습
* 'monologg/distilkobert' 기반
* Bert 뒤에 Classifier 추가. 결과가 토픽을 의미하는 9차원 원핫 벡터가 나오도록 학습했다.
* Accuracy = 0.9151 @ Epochs 5
* 체크포인트: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)
  (FalconNine = f9_bert, 폴더 구조도 유지해야 함)

## How to train / test
Train시, 모두의 뉴스 데이터셋을 준비해서 Root에 놓는다.  
|폴더 위치|데이터셋 내용|
|:---:|:---|
|`/internetnews`|인터넷 뉴스 (NIRW*.json)|
|`/smallnews`|인터넷 뉴스 앞 10개 (NIRW*.json 중 0001 ~ 0010)|

각 모델마다 작동 코드가 조금씩 다르므로, 구체적인 사항은 모델의 .ipynb를 확인해서 사용해야한다.  
각 Notebook 위쪽 섹션에는 세부 설정 코드가 있다. 모델 로드, 학습 및 테스트 코드는 "학습 모듈" 섹션 전후에 있다.  
일부 Notebook에서, 원한다면 `ModelContainer.train()` / `.test()` / `.test_single()` / `.load_from_file()` 사용하여 모델을 관리할 수 있다.  
자료를 전처리 하는 방법은 데이터셋 로드 섹션에 `nds_dataset` 초기화 시 `news_transform` / `date_transform` / `topic_transform` 에서 관리한다.  
일부 Notebook에서, Playground 섹션에 입력 가능한 단일 테스트용 코드가 있다.

## Final Report and Slides
Report: [Google Docs](https://docs.google.com/document/d/1-tK1bAfYEDU2q-fgNrBIrDbuqOnBG9guC3KSMoJgJ9I/edit?usp=sharing)  
Slides: [Google Slides](https://docs.google.com/presentation/d/149G_MRln6ZPB0GeJdo3sY7ejnp1wZFdv2iS2wJM6Zg8/edit?usp=sharing)
