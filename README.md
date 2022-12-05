# 한국어 혐오표현 데이터셋 교차 검증 연구(Cross-dataset test for improving hate speech detection models)

> **Author: Hayul Park (Korea Univ., major in Computational Linguistics)**

이 레포지터리는 `'혐오표현 탐지 모델의 개선을 위한 데이터셋 교차 검증'`이라는 제목으로 2023.02 출판 예정인 학위논문의 소스 코드와 실험 결과를 담고 있습니다. 

This repository contains experimental results and its source code of my master's thesis to be published on 2023.02, under the title `'Cross-dataset test for improving speech detection models'`.

## 초록

강건한(robust) 혐오표현 탐지 모델은 이전에 학습한 데이터와 다른 특징을 가진 혐오표현 데이터에도 일정한 분류 성능을 내는, 일반화가 잘 되는 모델이다. 그러나 현행의 모델 학습에 사용되는 데이터셋은 생성 방식, 주석 지침, 수집 플랫폼이 모두 달라서 어느 한 데이터셋을 잘 학습한 모델이더라도 다른 데이터셋에서는 성능이 상당히 달라진다. 이와 관련하여 외국에서는 몇몇 연구들이 혐오표현 탐지 모델의 일반화 가능성(generalizability)에 대해 선행적으로 논의를 시작하고 있으나, 아직 한국어 혐오표현 데이터셋과 이를 학습한 모델을 대상으로는 연구가 진행되지 않았다. 한국에서도 현재 온라인 혐오표현으로 많은 피해자가 생기고 있는 만큼, 강건하고 일반화가 잘 되는 혐오표현 탐지 모델이 시급하다. 따라서 본 연구는 한국어 혐오표현 데이터셋과 이를 학습한 모델의 일반화 가능성을 확인하고자 데이터셋 교차 검증(cross-dataset test)을 수행했다. 본 연구에서 사용한 데이터셋은 현재 공개되어 있는 데이터셋 중에 구축 지침이 명확한 4개의 데이터셋으로 한정하였다. 결과는 정확도와 F1 점수라는 정량적 지표와 함께, 정성적으로 각 모델과 데이터셋에 대한 오분류 사례를 제시했다. 실험의 결과로, [Moon 외 (2021)](https://github.com/kocohub/korean-hate-speech)의 데이터셋을 학습한 모델이 다른 데이터셋에서 비교적 성능 변화가 적어, 가장 일반화가 잘 되는 것을 확인하였다. 또한 양성 샘플의 비율이 큰 데이터셋으로 학습한 모델이, 양성 샘플의 비율이 큰, 다른 데이터셋에 테스트할 때 성능의 변화가 적은 것을 확인하여, 양성 샘플의 비율이 일반화 가능성과 관련된다는 것을 제시했다. 본 연구는 한국어 혐오표현 탐지 연구에 있어서 데이터셋 교차 검증을 통해 일반화 가능성에 대한 논의를 시작하였다는 의의가 있다. 

## Abstract
Machine learning models are robust when they generalize well to unseen data. However, hate speech detection models usually fail to generalize to similar but different hate contents, such as other hate speech datasets, as they differ in construction methods, annotation guidelines, and data sources from the data the model learned. A few have begun to discuss the generalizability of hate speech datasets, but it is only limited to the datasets written in English, while the generalizability of the Korean hate speech datasets has not yet been shed light. However, as victims of online hate speech are growing in Korea, a robust hate speech detection model is urgently needed. Therefore, this study examined the generalizability of the Korean hate speech datasets through cross-dataset test. I chose four publicly available datasets with clear construction guidelines and KcELECTRA model. I used them alternately in model training and testing and showed accuracy and F1 scores along with error examples for each pair. As a result, I found that the performance of the model trained with Moon et al. (2021) datasets changed relatively few on other datasets, meaning the model generalizes well to new data. In addition, I found that the model trained with a dataset with a large proportion of positive samples showed little change in performance when tested on other datasets with a large proportion of positive samples, meaning that the proportion of positive samples is important for generalizability. This study is meaningful as the starting point of the discussion on the generalization of Korean hate speech datasets. 

## 데이터셋(Datasets)

본 연구에서 사용한 데이터셋은 다음 4가지입니다.

- [Apeach](https://github.com/jason9693/APEACH)
- [Beep](https://github.com/kocohub/korean-hate-speech)
- [HateScore](https://github.com/sgunderscore/hatescore-korean-hate-speech)
- [Unsmile](https://github.com/smilegate-ai/korean_unsmile_dataset)

이상의 데이터셋의 정보는 다음과 같습니다.

| Datasets  | Method           | Columns                                                              | Hate  | Non-hate | Others | Hate  | Non-hate | Others |
| --------- | ---------------- | -------------------------------------------------------------------- | :---- | :------- | :----- | :---- | :------- | :----- |
| Apeach    | Crowd generation | text, user age, user gender, text topic, class, age, text topic(eng) | -     | -        | -      | 1,922 | 1,778    | -      |
| Beep      | Crawling         | comments, contain gender bias, bias, hate                            | 1,911 | 3,486    | 2,499  | 122   | 160      | 189    |
| HateScore | Mixed            | comment, macro label, micro label, source                            | 782   | 10,174   | 152    | -     | -        | -      |
| Unsmile   | Crawling         | 문장, 여성/가족, 남성, 성소수자, 인종/국적, 연령, 지역, 종교, 기타 혐오, 악플/욕설, clean, 개인 지칭   | 8,143 | 3,739    | 3,143  | 2,016 | 935      | 786    |

이상의 데이터셋을 실험을 위해 이진화하여 변환하면 아래와 같습니다. 

| Datasets  | Train/Hate   | Train/Non-hate | Test/Hate  | Test/Non-hate |
| --------- | :----- | :-- | :-- | :---- |
| Apeach    | -      | -        | 1,922 | 1,778    |
| Beep      | 4,410  | 3,486    | 311   | 160      |
| HateScore | 547    | 7,122    | 235   | 3,052    |
| Unsmile   | 11,266 | 3,739    | 2,802 | 935      |


## 혐오표현 탐지 모델 훈련(Hate speech detection model training)

**Hyper-parameters**
- model : KcELECTRA-base
- batch size : 64
- learning rate : 5e-5
- max_sequence_length : 128
- max_epoch : 10
- weight decay : 0.5
- optimizer : AdamW

### In-domain performance
모델의 테스트 데이터셋에서의 In-domain performance는 다음과 같습니다. 

| Model               | Accuracy  | F1 Score   |
| ------------------- | --------- | ---------- |
| Beep-KcELECTRA      | 0.81      | 0.83       |
| HateScore-KcELECTRA | 0.98      | 0.85       |
| Unsmile-KcELECTRA   | 0.89      | 0.92       |

## 데이터셋 교차 검증 결과(Cross-datasets test result)

### Out-of-domain performance
훈련 데이터셋(In-domain)이 아닌, 외부 데이터셋(Out-of-domain)에서의 데이터셋 교차 검증 결과는 아래 표와 같습니다.

| Metrics  | Models              | Apeach | Beep | HateScore | Unsmile |
| -------- | ------------------- | ------ | ---- | --------- | ------- |
| Accuracy | Beep-KcELECTRA      | 0.83   | -    | 0.60      | 0.85    |
| Accuracy | HateScore-KcELECTRA | 0.74   | 0.58 | -         | 0.73    |
| Accuracy | Unsmile-KcELECTRA   | 0.82   | 0.76 | 0.77      | -       |
| F1 Score | Beep-KcELECTRA      | 0.85   | -    | 0.25      | 0.90    |
| F1 Score | HateScore-KcELECTRA | 0.67   | 0.42 | -         | 0.79    |
| F1 Score | Unsmile-KcELECTRA   | 0.84   | 0.75 | 0.36      | -       |

### Out-of-domain difference
In-domain 테스트 데이터셋 대비, 외부 데이터셋(Out-of-domain)에서의 성능 차이(difference)는 아래 표와 같습니다.

| Metrics  | Models              | Apeach | Beep  | HateScore | Unsmile |
| -------- | ------------------- | ------ | ----- | --------- | ------- |
| Accuracy | Beep-KcELECTRA      | 0.02   | -     | -0.21     | 0.04    |
| Accuracy | HateScore-KcELECTRA | -0.24  | -0.40 | -         | -0.25   |
| Accuracy | Unsmile-KcELECTRA   | -0.07  | -0.13 | -0.12     | -       |
| F1 Score | Beep-KcELECTRA      | 0.02   | -     | -0.58     | 0.07    |
| F1 Score | HateScore-KcELECTRA | -0.18  | -0.43 | -         | -0.06   |
| F1 Score | Unsmile-KcELECTRA   | -0.08  | -0.17 | -0.56     | -       |

> To be updated...