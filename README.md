# OpenCV_Penguin_Project
OpenCV와 CNN 알고리즘을 활용한 펭귄 종 분류 모델

## 1.	주제 소개
수업시간에 배운 다양한 딥러닝 알고리즘을 활용하여 펭귄의 종을 분류하는 모델을 만들었다. 펭귄의 종은 아델리 펭귄, 턱끈펭귄, 황제펭귄, 갈라파고스 펭귄, 젠투펭귄, 쇠푸른펭귄, 마카로니 펭귄, 마젤란 펭귄, 바위 뛰기 펭귄 9종류로 선정했다. 펭귄의 종을 이렇게 설정한 이유는 펭귄의 대표적인 종들이기도 하며, 펭귄의 종을 하위 ‘속’으로 분류했을 때 각 ‘속’들에서 대표적인 종들이기 때문이다. 또한 각 9종류들은 서로 간의 특징이 명확히 구분되는 편이어서 모델링 작업을 하는 데 더 수월할 것이라고 생각했다. 


## 2.	데이터셋 구축
구글 이미지 크롤링을 통해 9종류의 펭귄 데이터를 모음. 파이썬 selenium 라이브러리를 사용했다. 키워드를 입력 받아 해당 키워드를 chrome 이미지에서 검색하여 얻을 수 있는 모든 이미지들을 로컬 폴더에 저장하고, 동일한 url의 이미지들은 중복 저장되지 않게 했다. 


![image](https://github.com/jasongoo827/OpenCV_Penguin_Project/assets/81475273/bf87ee22-1939-4dcd-94cd-6d7c09b37fab)



### 데이터 전처리

-  데이터 resize 128*128
  
데이터를 원본의 크기로 처리하게 되면 훈련을 시키는 데 있어 통일성이 떨어지게 된다. 또한 데이터의 크기가 지나치게 크다면 연산량이 많아지기 때문에 모델을 훈련할 때 시간이 너무 오래 걸리게 된다. 이러한 이유로 데이터의 크기를 128*128로 통일하여 모델의 계산 비용을 줄이고, 특징 추출을 강화하는데 도움을 주려고 했다. 


-  데이터 resize 후, blur 처리
  
이미지에 존재하는 잡음의 영향을 제거하고, 거친 느낌의 이미지를 부드럽게 처리하기 위해 blur 처리를 해 보았다. OpenCV의 Gaussian filter를 활용하여 전처리를 했다. 


-  데이터 resize 후, filter 처리
  
이미지의 중앙 픽셀을 강조하는 filter를 적용하여 전처리를 했다. 
```
  Mat kernel = (Mat_<float>(3,3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
```
이를 통해 중앙 영역의 픽셀들을 미세하게 강조하고 이미지의 선명도를 높이고자 한다. 
OpenCV의 filter2D 함수를 활용했다. 


-  데이터 resize 후, 샤프닝
  
언샤프 마스크 필터를 활용하여 이미지들을 전처리했다. 블러링이 적용되어 부드러워진 영상을 활용하여 날카로운 이미지를 만들었다. alpha 값을 곱한 원본의 이미지에서 Gaussian filter를 적용한 이미지를 빼서 전처리를 수행했다. 이를 통해 이미지의 윤곽이 뚜렷하고 선명한 느낌이 나도록 했다. 
```
  Mat img, blurred;
  GaussianBlur(img, blurred, Size(5,5), 1);

  float alpha = 1.f;
  Mat dst = (1+alpha) * img - alpha * blurred;
```

## 3. 프로젝트 결과

결과의 내용이 많기 때문에 따로 파일로 첨부하였다. 아쉬웠던 점은 시간이 부족해서 데이터를 많이 못했던 점과 교수님께서 결과가 잘 나오지 않을 때는 데이터를 잘 살펴보아야 한다고 조언해주셨었는데 그것을 잘 새겨듣지 않았던 점이다. 교수님께서 조언해주신 방향대로 프로젝트의 방향성을 잡았다면 더 좋은 결과가 나왔을 것이다. 여러모로 아쉬웠던 점이 많은 프로젝트였다. 


