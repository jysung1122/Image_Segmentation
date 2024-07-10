# Image_Segmentation
구글링을 통해 정리한 각각의 세그멘테이션 방식

### Image Segmantation
- 이미지 처리의 한 분야로, 이미지 내의 각 픽셀을 여러 클래스 중 하나로 분류하는 작업
- 객체 탐지는 바운딩 박스 단위로 인식을 하는 반면, 세그멘테이션은 바운딩 박스 안에 실제 객체의 영역까지 찾아낼 수 있음 ${\textsf{\textbf{\color{blue} => 큰 차이}}}$

## U-Net 기반의 세그멘테이션 24.07.09
- 모델의 구조가 U자임
- U-Net이라 불리는 인코더(다운샘플링)와 디코더(업샘플링)를 포함한 구조는 정교한 픽셀 단위의 segmentation이 요구되는 biomedical image segmentation task의 핵심 요소로 사용됨. 뿐만 아니라 세밀한 분류를 하기 위한 분야에도 사용됨
- Encoder-Decoder 구조 또한 semantic segmentation을 위한 CNN 구조로 자주 활용
  - ${\textsf{\textbf{\color{blue}Semantic segmentation: 이미지 속의 각 픽셀을 특정한 클레스에 할당하는 과정을 의미}}}$
  - ${\textsf{\textbf{\color{blue}이미지 속에서 같은 종류의 물체나 영역을 색칠하는 일종의 '자동 색칠하기' 기술}}}$
- Encoder 부분에서는 점진적으로 spatial dimention을 줄여가면서 고차원의 semantic 정보를 convolution filter가 추출해 낼 수 있게 함
  - ${\textsf{\textbf{\color{blue}이미지를 점점 더 작게 만들면서 중요한 특징들을 추출함. 이미지 크기는 줄어들고 각 블록의 정보의 중요도는 올라감}}}$
- Decoder 부분에서는 encoder 에서 spatial dimention 축소로 인해 손실된 spatial 정보를 점진적으로 복원하여 보다 정교한 boundary segmentation을 완성
- U-Net은 기본적인 encoder - decoder 구조와 달리 spatial 정보를 복원하는 과정에서 이전 encoder feature map 중 동일한 크기를 지닌 feature map을 가져와 prior로 활용함으로써(중요한 정보 재활용) 더 정확한 boundary segmentation이 가능하게 함 ${\textsf{\textbf{\color{red} => 아래 사진의 회색선}}}$
  - ${\textsf{\textbf{\color{blue}encoder 과정에서 얻은 중요한 정보(feature map)를 저장}}}$
  - ${\textsf{\textbf{\color{blue}decoder 과정에서 저장해둔 같은 크기의 feature map을 사용함}}}$
  - ${\textsf{\textbf{\color{blue}원래 이미지의 경계를 더 정확하게 복원 가능}}}$
 

![image](https://github.com/jysung1122/Image_Segmentation/assets/56614779/6da971d3-d601-48f6-b33f-17f3c8f3188a)


## DeepLap 기반 세그멘테이션 24.07.10
- 버전이 업그레이드 되면서 발전되고 있음

## DeepLab V1 
- Atrous convolution 이라는 개념이 도입됨
  - ${\textsf{\textbf{\color{blue}기존 convolution과 다르게 필터 내부에 빈 공간을 둔 채로 작동}}}$
- 기존 convolution과 동일한 양의 파라미터와 계산량을 유지하면서 field of view (한 픽셀이 볼 수 있는 영역)를 크게 가져갈 수 있음
  - ${\textsf{\textbf{\color{blue}기존 convolution에서는 작은 필터가 작은 영역을 보면서 이미지를 처리함}}}$
  - ${\textsf{\textbf{\color{blue}Atrous convolution은 필터 안에 빈 공간을 두어, 같은 크기의 필터로도 더 넓은 영역을 볼 수 있게 함}}}$
  - ${\textsf{\textbf{\color{blue}=> 필터의 수나 계산량은 그대로 유지되면서도, 이미지를 더 넓게 볼 수 있어 더 많은 정보를 얻을 수 있음}}}$
- Semantic segmentation에서 일반적으로 높은 성능을 내기 위해서는 convolutional neural network의 마지막에 존재하는 한 픽셀이 입력값에서 어느 크기의 영역을 커버할 수 있는지를 결정하는 receptive field 크기가 중요
  - ${\textsf{\textbf{\color{blue}CNN에서 마지막 계층의 각 픽셀은 이전 계층의 여러 픽셀들을 결합하여 정보를 얻음}}}$
  - ${\textsf{\textbf{\color{blue}이때, receptive field는 이 픽셀이 원래 이미지에서 커버하는 영역의 크기를 나타냄}}}$
  - ${\textsf{\textbf{\color{blue}넓은 receptive field를 가지면, 더 넓은 범위의 정보를 고려할 수 있어 이미지의 복잡한 특징을 더 잘 포착할 수 있음}}}$
- Atrous convolution을 활용하면 파라미터 수를 늘리지 않으면서도 receptive field를 크게 키울 수 있기 때문에 DeepLab 계열에서는 이를 적극적으로 활용

![image](https://github.com/jysung1122/Mini_Project_Secret_Auction/assets/56614779/aa4c3416-eaac-4007-8a8f-bf1e7d6dc1d0)
