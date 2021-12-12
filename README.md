# SanctiMoly
Neural network models for predicting the date / topic of news

## Dataset
[국립국어원: 모두의 말뭉치](https://corpus.korean.go.kr/) 사이트에서 신문 말뭉치 자료를 사용함.

모델 학습 시, Root에 데이터를 두 폴더에 넣어야함.
`/internetnews` 인터넷 뉴스 (NIRW*.json)
`/smallnews` 인터넷 뉴스 앞 10개 (NIRW*.json 중 0001 ~ 0010)

[Korpora가 제공하는 라이브러리](https://ko-nlp.github.io/Korpora/)를 활용하여 데이터를 불러옴.

## Models
일부 모델의 경우 /runs 폴더 내에 Tensorboard Log가 존재함.
모델의 체크포인트의 용량이 큰 관계로, 공유된 링크를 통해 따로 다운받아야 함.

### ToTheMoon (Date)
'smallnews' 학습; 'monologg/distilkobert' 기반, bert 뒤에 Classifier 붙임. Bert는 params freeze. Bert [CLS] token output을 Classifier에 넘겨주는 형태. 뉴스 -> 날짜 0~120 1차원
768 > Linear > 1024 > BN1d > Tanh > Dropout(p=0.5) > 1024 > Linear > 768 > BN1d > Tanh > Dropout > 768 > Linear > 1
accu = 0.00994 @ epochs 3
(저장된 Checkpoint 없음)

### BTTtoMars (Date)
'smallnews' 학습; 'monologg/distilkobert' 기반, bert 뒤에 Classifier 붙임. Bert는 params freeze. Bert [CLS] token output을 Classifier에 넘겨주는 형태. 뉴스 -> 날짜 120 One-hot
768 > Linear > 1024 > BN1d > Tanh > Dropout(p=0.5) > 1024 > Linear > 768 > BN1d > Tanh > Dropout > 768 > Linear > 120
accu = 0.3646 @ epochs 20
다음 경로에 파일 넣기: /nBTT_20_ckpts/[Checkpoint files](https://drive.google.com/drive/folders/1nTLRIxLlKlmalRsgst69UkH0peiu1xxm?usp=sharing)

### BARDE_Alpha (Date)
'smallnews' 학습; 'gogamza/kobart-summarization' 기반, 인코더와 임베딩 층 params freeze. 뉴스 -> 날짜 문자열 생성. 15 epochs 학습
accu = 0.2899 @ epochs 14
다음 경로에 파일 넣기: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)

### BARDE_Beta (Date)
'internetnews' 학습; 모델 구조는 BARDE_Alpha랑 같음. 10 epochs 학습
accu = 0.3532 @ epochs 10
다음 경로에 파일 넣기: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)

### FalconNine (Topic)
'internetnews' 학습; 'monologg/distilkobert' 기반, bert 뒤에 Classifier 붙임. Bert는 params freeze 아님.
뉴스 -> 토픽 One-hot (9차원)
Bert > 768 > Linear > 768 > Linear > 9 > Dropout(p=0.2)
accu = 0.9151 @ epochs 5
다음 경로에 파일 넣기: /models/[Checkpoint files](https://drive.google.com/drive/folders/1-0pzqbjlZ9hci2c9J6JVHp5MFENZ97fQ?usp=sharing)
(FalconNine = f9_bert)

## How to train / test
Train시, 위 Dataset 섹션에서 적힌 대로 모두의 뉴스 데이터셋을 준비해야함.
각 모델마다 작동 코드가 조금씩 다르므로, 구체적인 사항은 모델의 .ipynb를 확인해서 사용해야함.
각 Notebook 위에는 세부 설정 코드가 있음. 모델 로드, 학습 및 테스트 코드는 "학습 모듈" 섹션 전후에 있음.
ModelContainer.train() / .test() / .test_single() / .load_from_file() 사용함
자료를 전처리 하는 방법은 "데이터셋 로드" 섹션에 news_transform / date_transform / topic_transform 참조
각 Notebook의 Playground 섹션에 일부 모델의 경우 입력 가능한 단일 테스트용 코드가 있음.

## Final Report and Slides
Report: [Google Docs](https://docs.google.com/document/d/1-tK1bAfYEDU2q-fgNrBIrDbuqOnBG9guC3KSMoJgJ9I/edit?usp=sharing)
Slides: [Google Slides](https://docs.google.com/presentation/d/149G_MRln6ZPB0GeJdo3sY7ejnp1wZFdv2iS2wJM6Zg8/edit?usp=sharing)
