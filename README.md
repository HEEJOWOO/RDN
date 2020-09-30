# RDN : https://arxiv.org/abs/1802.08797

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Residual Dense Network for Image Super-Resolution
--------------------------------------------------------------------------

* SR DenseNet을 개선
* Residual Dense Block(RDB)
  * Dense Connected Convolutional layers
  * 많은 local feature 추출
* Contiguous memory(CM)
  * 이전 RDB output 현재 RDB Conv layer 연결
* Local Feature Fusion(LFF) 
  * 이전의 특징과 현재의 특징을 효과적으로 학습
  * 넓은 Network을 Training할 때 안정적으로 학습
* Global Feature Fusion(GFF)
  * 계층적인 특징들을 학습

* DenseNet과의 차이점
  * High-level computer vision에 사용 / RDN : Img SR에 사용
  * Batch normalization 제거 
  * Pooling layer 제거  
  * 계층적 특징들을 사용하기 위한 Global feature fusion
  
* SR DenseNet 과의 차이점
  * 이전 RDB의 output을 현재 RDB의 Conv layer에 모두 사용하는 CM 사용
  * Local Feature Fusion 사용, 높은 Growth rate

* MemNet 과의 차이점
  * MemNet의 Memory block끼리 접근 할 수 없음
  * Local feature의 정보를 완벽하게 사용할 수 없음
  * MemNet : ILR 사용 / RDN : LR 사용
 
RDN Architecture
-----------------
![image](https://user-images.githubusercontent.com/61686244/94661523-221d4300-0342-11eb-8fec-570c89dc9a56.png)
![image](https://user-images.githubusercontent.com/61686244/94662154-f77fba00-0342-11eb-8ab9-70a9f647e37e.png)
 * Shallow Feature Extraction Net
    * 두개의 Convolution으로 구성 돼 있으면 얕은 특징을 추출하는 단계로 이루어져있음
    * 첫 번째 Conv를 통과해서 나온 F-1은 두 번째 Conv의 input이기도 하지만 Dense Feature Fusion에 사용된 Global residual learning의 input 값으로도 사용
    * 두 번째 Conv를 통과해서 나온 F0도 얕은 특징을 추출하는 단계이며 RDB에 input으로 들어가는 기능 수행

* Residual Dense Blocks(RDBs)
  * Global feature fusion을 보게 되면 각 RDB로부터 나온 특징들을 융합을 하여 FGF를 만드는 단계
  * 1x1 conv와 3x3 conv를 지나가게 되는데 1x1 conv는 앞에서 봤더 각 RDB마다 적용을 했던 1x1과 동일한 작용을 하며 따라서 채널의 수를 제어하는 역할을 함
  * 3x3 conv 같은 경우에는 앞의 F-1의 global residual learning을 위해 특징을 추가로 추출하기 위해 사용
  * RDB로 부터 나온 여러 level의 특징들은 FGF형태를 띄우고 후에 global residual learning 와 dense feature fusio을 통해 Fdf를 만들 수 있음


* Dense Feature Fusion(DFF)
    * DFF단계는 RDB를 통해서 나온 global feature fusion과 앞에서 첫 번째 conv를 통해서 나온 global residual learning, GRL을 포함하고 있는 단계

* Up-Sampling Net(UPNet)
    * ESPCN논문에서 선보인 sub pixel convolution을 사용하여 채널 별 셔플을 통해 up sampling을 진행
    
Different values of D, C, G
---------------------------
![image](https://user-images.githubusercontent.com/61686244/94675231-5e599f00-0354-11eb-9f3f-066b10444706.png)
* Train Set : DIV2K / Test Set : Set5, Set14, Urban100, Manga109 

Different combinations of CM, LRL, and GFF
------------------------------------------
![image](https://user-images.githubusercontent.com/61686244/94675319-7fba8b00-0354-11eb-9b7a-e1310be2542b.png)
![image](https://user-images.githubusercontent.com/61686244/94675337-85b06c00-0354-11eb-81ce-4cd0dd75a257.png)

BI degradation model
--------------------
![image](https://user-images.githubusercontent.com/61686244/94675441-b42e4700-0354-11eb-90aa-e8b86f7bf2ff.png)

BD degradation model & Bicubic downsample model
-----------------------------------------------
![image](https://user-images.githubusercontent.com/61686244/94675517-d1fbac00-0354-11eb-9c7d-62de7c903b45.png)
![image](https://user-images.githubusercontent.com/61686244/94675684-10916680-0355-11eb-9c6c-6b7bd4664f81.png)




