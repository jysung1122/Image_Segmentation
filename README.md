# Image_Segmentation
구글링을 통해 정리한 각각의 세그멘테이션 방식

### 24.07.09

### Image Segmantation
- 이미지 처리의 한 분야로, 이미지 내의 각 픽셀을 여러 클래스 중 하나로 분류하는 작업
- 객체 탐지는 바운딩 박스 단위로 인식을 하는 반면, 세그멘테이션은 바운딩 박스 안에 실제 객체의 영역까지 찾아낼 수 있음 ${\textsf{\textbf{\color{blue} => 큰 차이}}}$

## U-Net 기반의 세그멘테이션 
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

### 24.07.10

## DeepLap 기반 세그멘테이션 
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

## DeepLab V2
- Semantic segmentaion의 성능을 높이기 위한 방법 중 하나로, spatial pyramid pooling(SPP) 기법을 자주 사용
  - ${\textsf{\textbf{\color{blue}SPP}}}$
    - ${\textsf{\textbf{\color{blue}이미지를 다양한 크기의 그리드로 나누어, 각 그리드에서 feature를 추출한 다음, 이 feature들을 결합하여 최종 feature map을 만드는 기법}}}$
    - ${\textsf{\textbf{\color{blue}이미지의 특정 부분에서 더 많은 정보를 얻어낼 수 있으며, 이미지 크기에 구애받지 않고 일정한 크기의 feature map을 만들 수 있음}}}$
    - ${\textsf{\textbf{\color{blue}이미지 내의 다양한 스케일에서 정보를 효과적으로 추출할 수 있게 해줌}}}$
- Feature map으로부터 여러 개의 rate가 다른 atrous convolution을 병렬로 적용한 뒤, 이를 다시 합쳐주는 atrous spatial pyramid pooling (ASPP) 기법을 활용
  - ${\textsf{\textbf{\color{blue}ASPP}}}$
    - ${\textsf{\textbf{\color{blue}여러 개의 atrous convolution(빈 공간이 있는 필터)을 벙렬로 적용}}}$
    - ${\textsf{\textbf{\color{blue}각 필터는 다른 rate(간격)를 가지고 있어서, 각각 다른 크기의 receptive field를 만듦}}}$
    - ${\textsf{\textbf{\color{blue}이렇게 하면, 이미지의 다양한 스케일에서 정보를 추출할 수 있음}}}$
    - ${\textsf{\textbf{\color{blue}마지막으로, 다양한 크기의 정보를 하나로 합쳐서 최종 feature map을 만듦}}}$
    - ${\textsf{\textbf{\color{blue}이는 이미지의 여러 스케일에서 중요한 특징을 모두 포착할 수 있게 해줌}}}$
    - ${\textsf{\textbf{\color{blue}=> 큰 사진을 배율이 다른 여러 돋보기를 통해서 본다고 생각!}}}$
- multi-scale context를 모델 구조로 구현하여 보다 정확한 semantic segmentation을 수행할 수 있도록 함

![image](https://github.com/jysung1122/Mini_Project_Secret_Auction/assets/56614779/23fc609c-30ce-474a-a092-def9abcd6935)

## DeepLab V3
- Encoder: ResNet with Atrous convolution
  - ${\textsf{\textbf{\color{blue}ResNet(Residual Network)은 깊은 신경망에서 발생할 수 있는 문제들을 해결하기 위해}}}$ ${\textsf{\textbf{\color{red}잔차연결}}}$ ${\textsf{\textbf{\color{blue}을 사용하는 네트워크}}}$
  - ${\textsf{\textbf{\color{blue}=> ResNet은 책을 읽는 방식, Atrous convolution은 돋보기를 사용해서 보는 것}}}$
    - ${\textsf{\textbf{\color{blue}-> 책의 세부사항과 전체적인 내용을 동시에 잘 이해할 수 있음}}}$
  - ${\textsf{\textbf{\color{red}잔차연결 : 네트워크의 일부 출력을 네트워크의 더 나중 단계에 다시 추가하는 방법 -> 깊은 신경망에서 정보가 더 잘 전달되고, 학습이 더 쉬워짐}}}$
    - ${\textsf{\textbf{\color{red}=> 깊은 신경망에서는 각 계단을 오르면서 새로운 것을 배워야 함}}}$
    - ${\textsf{\textbf{\color{red}=> 잔차연결은 계단의 중간에 쉬는 플랫폼을 둬, 올라가면서 이전 계단의 정보를 다시 확인할 수 있게함 -> 계단을 오르기가 훨씬 쉬워짐}}}$
- Atrous Spatial Pyramid Pooling (ASPP)
- Decoder: Bilinear Upsampling
  - ${\textsf{\textbf{\color{blue}Bilinear Upsampling : 이미지를 더 크게 만드는 방법. 단순히 픽셀을 복사해서 이미지를 확대 X. 주변 픽셀들의 값을 이용해 부드럽게 확대}}}$
 
![image](https://github.com/jysung1122/Mini_Project_Secret_Auction/assets/56614779/c450e555-5f6c-4aa7-ab34-7ca5ab05a18d)

## DeepLab V3+
- Encoder: ResNet with Atrous Convolution → Xception (Inception with Separable Convolution)
  - ${\textsf{\textbf{\color{blue}Inception 구조 : 다양한 크기의 필터를 병렬로 사용하여 이미지 분석. 즉, 이미지의 여러 부분을 동시에 살펴봄}}}$
    - ${\textsf{\textbf{\color{blue}=> 사진을 찍을 때 여러 개의 카메라 렌즈를 동시에 사용하여 다른 각도와 초점을 맞추는 것과 비슷함}}}$
  - ${\textsf{\textbf{\color{blue}Separable convolution : 필터를 두 단계로 나눠 사용하는 방법. 공간적으로 먼저 분리하고, 그 다음 채널 별로 분리함 -> 계산량을 줄이면서도 성능을 높일 수 있음}}}$
    - ${\textsf{\textbf{\color{blue} => 큰 그림을 그릴 때, 가로선과 세로선을 따로 그려서 더 빠르고 효율적으로 완성하는 것과 같음}}}$
- ASPP → ASSPP (Atrous Separable Spatial Pyramid Pooling)
  - ${\textsf{\textbf{\color{blue}Atrous convolution, Separable convolution, Spatial pyramid pooling}}}$
  - ${\textsf{\textbf{\color{blue}위 세가지 기법을 결합하여, 이미지의 다양한 크기에서 중요한 정보를 추출하고 이를 하나로 합쳐 더 정확한 결과를 얻음 -> 성능을 높이면서도 계산량을 효율적으로 관리할 수 있음}}}$
- Decoder: Bilinear Upsampling → Simplified U-Net style decoder
  - ${\textsf{\textbf{\color{blue}이미지를 작은 블록으로 나눠서 중요한 정보를 추출한 후, 다시 원래 크기로 키우는 과정에서 각 단계에서 얻은 중요한 정보를 계속 사용함 -> 이미지를 더 정확하게 복원 가능}}}$

![image](https://github.com/jysung1122/Mini_Project_Secret_Auction/assets/56614779/2f8ba7c4-5bcd-4074-b347-17fdf5da7519)


![image](https://github.com/jysung1122/Mini_Project_Secret_Auction/assets/56614779/9c1503a4-ddf5-4bd4-b807-ce5d9407543d)









