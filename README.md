# SanctiMoly
Bert based model for predicting the date of the news

## Dataset
국립국어원: 모두의 말뭉치 사이트에서 신문 말뭉치 자료 사용

https://corpus.korean.go.kr/

Korpora가 제공하는 라이브러리 초기에 활용하여 데이터로드

https://ko-nlp.github.io/Korpora/
속도 및 처리에 개선사항이 필요해 직접 자료 전처리 후 데이터로더 제작

## Model
BTTtoMars : Unfreezed BERT, 120 one-hot encoding, only 10 sets from dataset
XLPtoSun : Unfreezed BERT, 180 one-hot encoding, weighted loss(computed from total data), only 10 sets from dataset
