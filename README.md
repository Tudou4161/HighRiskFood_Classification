# 전이학습을 활용한 고위험군 음식 분류

## 프로젝트 개요
<p> 코로나 바이러스 및 여름철 식중독의 위험으로 인해, 고위험군 음식과 저위험군 음식을 구별할 필요성이 커지고 있다. 따라서 이미지만 보고 고위험군 음식을 분류할 수 있는 딥러닝 모델을 구축해봤다.</p>

## 기술 스택
<ul>
  <li>Sklearn</li>
  <li>Tensorflow, keras</li>
  <li>Numpy</li>
  <li>requests</li>
</ul> 

## 프로젝트 프로세스
1. 크롤링 및 데이터 준비 
<p> requests, bs4라이브러리를 활용하여 고위험군에 속하는 음식(박쥐탕, 뱀탕) 저위험군에 속하는 음식(짜장면, 제육볶음, 김치찌개)을 음식별로 100장씩 수집했다. 이후 수집된 500장의 이미지 데이터를 data폴더에 Train(60%)/validation(20%)/Test(20%) 하위 폴더들에 적재했다. </p>
<a href="https://github.com/Tudou4161/HighRiskFood_Classification/blob/main/HCFC_crawling_data.ipynb">크롤링 소스코드 보러가기</a>
<br />

2. 모델링
<p> 데이터의 수가 매우 적은 편이기 때문에, 데이터를 증식시킬 필요가 있었다. 따라서 keras의 ImageDataGenerator모듈을 활용하여 데이터를 증식시켰다.
    이후, 증식된 데이터를 바탕으로 필자가 직접 구성한 CNN코드와 전이학습 알고리즘 중 하나인 VGG19를 사용하여 데이터를 학습시켰다.</p>
<br />
<p>CNN을 활용하여 학습시킬 때, 과적합을 방지하기 위해 여러 규제들을 적용했다. DropOut과 L2규제, 배치정규화를 도입하여 과적합을 최소화하려 했다. 
    결론적으로, 과적합은 방지할 수 있었지만 80%이상의 좋은 성능을 보여주진 못했다. 학습시킬 때부터 70% 중후반대를 유지했고, 테스트 때도 마찬가지로 70%대를 맴돌았다. </p>
<a href="https://github.com/Tudou4161/HighRiskFood_Classification/blob/main/HRFC_Simple_CNN_72.ipynb">CNN 소스코드 보러가기</a>
<br />
<p>CNN에서의 미진한 정확도를 개선하기 위해, 전이학습 알고리즘 중 하나인 VGG19를 도입했다. 미세조정(Fine-Tuning)없이 학습시킨 결과 테스트 정확도가 80-90%를 웃돌았다.
    하지만 매번 새로 학습을 시킬 때마다 정확도 추이가 최대 +-10% 를 기록할만큼 변동추이가 컸다. </p>
<a href="https://github.com/Tudou4161/HighRiskFood_Classification/blob/main/HRFC_VGG19_Adam_84.ipynb">Non-Fine-Tuned-VGG19 소스코드 보러가기</a>
<br />
<p>마지막으로, VGG19에 미세조정을 도입해서 학습을 진행했다. block5_conv1 계층부터 block5_poll 계층까지 파라미터를 재학습시켰다. 그 결과 학습 시 정확도 90%중후반, 테스트 정확도 91%를
기록할 수 있었다.</p>
<a href="https://github.com/Tudou4161/HighRiskFood_Classification/blob/main/HRFC_VGG19_FineTuning_91.ipynb">Fine-Tuned-VGG19 소스코드 보러가기</a>
<br/>

## 프로젝트 평가
<ul>
  <li>class가 5개 밖에 없어서 모델링이 수월했다. 하지만 음식 종류는 수 천 수 백만 가지이다. 이 종류들 중에서 고위험군 음식을 식별한다는 것은 사실상 불가능한 일일지 모른다.<br />
     class가 기하급수적으로 많아진다면... 정확도가 크게 떨어질지도 모른다. </li>
  <li>본 프로젝트는 어느정도 편향성을 갖고 있다. 특정 음식을 섭취하는 것이 코로나 바이러스 감염 위험성을 증가시킨다는 객관적 연구결과도 없는 상태이다.<br />
      하지만, 코로나 바이러스 뿐만 아니라 식중독 및 해외에서의 불분명한 음식 취식 등 고위험군 음식 관련 이슈들은 다양하게 존재한다. 따라서 모델 성능이 더 좋아진다면,
      충분히 유익한 모델이 될 수 있을 것이라 생각한다.</li>
</ul> 
