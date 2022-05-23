# 한국어 혐오표현 데이터셋 Cross-dataset-Testing

최근 혐오표현 탐지에 대한 관심이 모임에 따라 다양한 혐오표현 데이터셋이 발표되었습니다. 그러나 데이터셋 간의 `일반화 가능성(generalizability)`을 보는 연구는 아직 수행된 바가 없습니다.
이에 저는 [Smilegate.AI](http://Smilegate.AI)에서 배포한 `Unsmile` 데이터셋을 이용해 언어 모델을 `classification task`로 fine-tuning하고, 훈련시킨 모델을 다른 혐오표현 데이터셋에 테스트해보는 “Cross-Dataset Testing”을 개인 프로젝트로 진행했습니다. 구체적으로, `Unsmile` 데이터셋에 훈련된 모델이 `한국어 혐오표현 데이터셋`(https://github.com/kocohub/korean-hate-speech) 과 `HateScore`(https://github.com/sgunderscore/hatescore-korean-hate-speech) 데이터셋을 __어느 정도로 정확히__ 예측하는지 실험했습니다. 

실험은 `Colab pro GPU` 환경에서 진행되었습니다. 실험 과정은 다음과 같습니다. 먼저 `Unsmile` 데이터셋은 여러 사회적 소수자에 따른 `label`이 존재하므로, 이를 하나로 합쳐서 'hate’ column을 만들고 해당 문장이 혐오표현이면 ‘1’, 아니면 ‘0’을 기입했습니다. 나머지 두 데이터셋 또한 같은 방식으로 ‘hate’ column을 만들었습니다. 이후 Unsmile 데이터셋은 train/test 셋으로 0.7/0.3 비율로 분할하였습니다.  

다음으로 프레임워크로 `Tensorflow`를 사용하였고, 모델은 huggingface를 통해 `KoBERT`를 불러왔습니다. `BATCH SIZE`는 32로 설정했습니다. 활성화 함수로 `sigmoid`를 사용하였고, `Learning rate`는 5.0e-5, 평가 척도는 `accuracy`로 설정했습니다. 이후 5 에포크 동안 훈련시킨 결과, `accuracy`는 0.90으로 계산되었습니다. 

훈련시킨 모델을 `한국어 혐오표현 데이터셋`에 테스트한 결과, `accuracy`는 0.73이 계산되었으며, `HateScore`에 테스트한 결과, `accuracy`는 0.64가 계산되었습니다. 
__이를 통해 세 데이터셋 간의 일반화 가능성(generalizability)이 약하다는 것을 확인하였습니다.__

__한국어 혐오표현 데이터셋 Confusion Metrix__

![CM]("./[CM]Beep_predicted_by_Unsmile.png")

