## Rendering Pipline or Graphics Rendering Pipline
이 과정은 3차원으로 만들어진 모델을 2차원에 투영하는 Rendering Process를 자세하게 표현한것이다.
컴퓨터에 데이터로 존재하는 3D 리소스가 모니터에 출력되는 과정이 Rendering Pipline을 따르게 된다.

![image](https://github.com/showhohxc/ProjectWill/assets/98040028/458793c1-6f87-49ad-b014-f5edc6af530e)

위의 과정을 따라 화면에 도형을 표시할 수 있으며, 주황색 박스의 과정은 조작이 가능한 (Programmable) 단계이다.

## Input Assembler
Vertex Specfication (꼭지점 사양?) 이라 불리는 Input Assembler 단계는 CPU로부터 수행할 도형의 정점 정보를 정점 버퍼에 담아 전달 받는다.
GPU는 CPU 자원에 접근이 불가 하므로 CPU에서 버퍼라고 부르는 메모리에 필요한 정보를 담아 GPU로 전달해준다. [GPU->CPU : X | CPU->Buffer->GPU ]

## Vertex Shader
Input Assetbler 단계에서 정점버퍼에 담긴 데이터 정점들의 정보로 도형은 생성되었지만 공간 좌표계를 변환할 필요가 있다.
기본적으로 도형들은 자신만의 좌표계인 Local Space 좌표를 가지고 있는데, 모든 물체들이 하나의 월드에 위치하도록
Local Space에서 World Space로 변환하고, 실제 플레이어가 바라보는 카메라가 중심이 되는 공간이 View Space로 변환해준다.

## Tesselation 
Tesselation 단계는 주어진 모델의 정점을 더 잘게 쪼개어 디테일하게 표현할때 사용한다.
일반적으로 가까운 거리의 물체는 고해상도로, 먼거리의 물체는 저해상도로 표현하여 성능을 향상시키는데,
고해상도로 표현되었을 때 보여져야 할 텍스처 형태를 높이 맵으로 저장해놓고 실제 모델에 필요한 만큼의 레벨만큼만 적용하는 식으로 Tesselation을 활용하면 하나의 모델에 대해
< 여러 해상도의 모델 > 데이터를 가지고 있을 필요가 없다.

이는 LOD(Level Of Detail)을 적용하여 여러 해상도로 표현 가능한 모델을 마련했는데, 그 모델들끼리의 부자연스러운 경우를 Tesselation으로 자연스럽게 표현할 수 있는 것이다.

## Geometry Shader
기본 도형에서 정점을 추가하거나 삭제하여 모델을 변경할 수 있는 Shader이다. 
Gemotry Shader 정점 정보를 조금 추가하여 표현할 수 있는 모델이라면 그만큼의 정점 정보를 빼고 저장 할 수 있으니
디스크 용량과 그래픽 메모리 절약에 도움이 될 수도 있으며, Tesselation 등으로 추가된 정점들을 표현할 때도 사용한다.

## Rasterization
정점 정보를 완전히 결정한 3D 도형을 실제 픽셀 데이터로 변환해주는 단계이다.
이 때 우리가 가진 정점 데이터는 말그대로 [정점 데이터 이고], 정점 사이의 공간은 보간(Interpolation)을 통해 메워주어야 한다.

## Fragment Shader
Pixel Shader 라고도 부르며, 래스터된 도형에 텍스처 매핑, 범프 매핑, 노말 매핑 등의 기법으로 
텍스처을 입혀 색을 표현한다. 또한, 정점의 법선 벡터 정보를 통해 조명처리도 해당 셰이더에서 이루어진다.

