# FreshFruits-ImageClassification
----
## 문제 인식 및 가설 설정
- 스마트 농업은 농업 밸류체인(생산과 유통, 소비) 전반에 첨단 정보통신기술이 접목되어 자동화, 지능화를 구현하는 개념이고, 디지털 농업은 농업 관련 전반의 데이터를 디지털화하여 수집, 분석하고 공유하는 기술의 의미합니다. [출처: 한국과학기술기획평가원(KISTEP) 기술동향브리프 | 2021-03호] 벨기에 옥타니온 딸기 수확 로봇 루비온, 미국 루트 AI의 방울 토마토 수확 로봇 비르고처럼 인공지능을 활용한 농업 산업이 계속 발전을 이루고 있는 상황입니다.    
- 신선한 과채 분류 모델이 있는 경우, 생산과정에서는 신선한 과채을 수확하거나 신선하지 못한 과채의 제거에 이용할 수 있고 유통과정에서는 수확 후 포장과정 자동화나 유통 중 품질관리에 이용할 수 있습니다. 현실적으로 다양한 과채가 존재하기 때문에, 과채 종류별로 모든 모델을 만들기에는 한계가 있으므로 주요 과채모델을 만든 후 그 모델을 다른 과채에 적용 가능한지 검토해 보려고 합니다.
- 이 프로젝트는 먼저 사과 이미지 분류 모델을 개발합니다. 그리고 형태가 비슷한 과일(배, 오렌지, 복숭아)과 형태가 비슷하지 않은 과일(바나나, 파인애플)의 신선 여부 판단에 있어서 사과 분류 모델 적용시 과일별로 성능 차이가 존재하는지 검토하고자 합니다.

---
## 프로젝트 진행 과정
- 데이터 전처리 : 이미지 분류 모델로 특별한 전처리가 없음
- 사과 분류 모델 개발 : 사전 학습모델을 활용한 딥러닝 기법 활용
   - 사과 분류 모델을 이용한 오렌지 분류, 바나나 분류 테스트
   - Train : fresh apples, rotten apples로 모델학습 진행
   - Test : apples, banana, oranges 모델 성능 테스트 진행
- 과일 분류 모델 개발
   - Train : (fresh/rotten) apples, banana, oranges 모두 활용하여 모델학습 진행
   - Test : apples, banana, oranges 모델 성능 테스트 진행

---
## 데이터셋 설명
- 캐글 원본 데이터 : Fruits fresh and rotten for classification     
  (https://www.kaggle.com/datasets/sriramr/fruits-fresh-and-rotten-for-classification)
      
    ![image](https://user-images.githubusercontent.com/109954540/227492626-89d7ae9d-f7b6-425e-a837-046af6161bcd.png)

---

## 분류모델 학습 및 성능 확인
- 분석 기법 : 신선한 과채 여부를 확인하기 위해 전이학습을 통한 딥러닝 기법을 활용하여 이진 분류 모델을 학습
   - 사전 학습모델 및 활용할 파이썬 라이브러리 : tensorflow.keras
       - 사전 학습모델 : Resnet50, VGG16, InceptionV3, EffecientNetB7
       - 완전 연결신경망(epochs=30 고정) : Case1(dense = 256), Case2(dense=64, dense = 128, dropout=0.2)
- 사과 분류 모델 학습 성능 확인
    - 사과 분류 모델 : 사전학습모델 VGG16과 InceptionV3의 정확성이 높음
    - 사과 분류 모델을 활용한 다른 과채 분류 테스트 : 바나나와 오렌지의 분류 성능에 큰 차이는 없음
        - Train : fresh apples, rotten apples로 모델학습 진행
        - Test : apples, banana, oranges 모델 성능 테스트 진행   
        
        ![image](https://user-images.githubusercontent.com/109954540/227495364-def449f8-bb33-4ea5-a68a-b742699b5df4.png)

- 과일 분류 모델 학습 성능 확인 
   - 사과 분류 모델에서 정확성이 높았던 VGG16과 InceptionV3 사용 
       - Train : (fresh/rotten) 과일들을 다양하게 조합
           - Apples + Banana (total : 7,840)
           - Apples + Oranges (total:7,096)
           - Apples + Banana + Oranges (total : 10,901)
       - Test
           - Apples (total : 996)
           - Banana (total : 911)
           - Oranges (total : 791)    
           
       ![image](https://user-images.githubusercontent.com/109954540/227497812-000b7a05-bdd6-4289-b274-8e032945059a.png)

---
## 결과 정리
- 사전 학습 모델의 성능테스트
    - epochs=5 정도에서 윤곽이 형성되므로 epochs=30까지는 불필요
        - Resnet50의 경우 epochs=100 -->정확도 0.85수준
        - VGG16, InceptionV3의 경우 epochs=5 --> 정확도 0.9 이상
     - Case1(dense = 256), Case2(dense=64, dense = 128, dropout=0.2)의 성능차이가 미세하므로 이중 성능 확인 불필요
- 프로젝트 아이디어
    - 형태가 비슷한 과일(배, 오렌지) 과 형태가 비슷하지 않은 과일(바나나, 파인애플)을 사과 분류 모델 적용시 성능차이가 존재
- 프로젝트 결과
    - 과일 형태에 따른 성능차이가 존재하지 않음
    - 가설 검정 : 사과 분류 모델을 이용하여 오렌지 분류가 가능하다는 가설은 기각됨

---
## 프로젝트 한계점  
- 굉장히 높은 정확도가 있는 분류 모델을 개발하였지만, 이는 과채 중심의 이미지를 사용하였기 때문으로 판단되므로 주변 배경과 함께 있는 이미지를 포함한 경우에도 분류 모델의 정확도가 높은지 추가로 확인할 필요가 있음
- 현실에서의 유용성 증대를 위해서는 영상을 통한 과채분류 모델학습이 필요함
- 이번 프로젝트에서는 가설이 기각되었지만, 하나의 과채가 아니라 다양한 과채를 포함한 훈련데이터를 활용하여 과채의 형태 또는 색채에 따른 분류모델을 모델링 하는 것이 가능한지 여부에 대한 검토를 더 진행할 수 있다고 생각함



