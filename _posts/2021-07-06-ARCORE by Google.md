---
layout: single
title : "[Android] ARCORE by Google"
categories : [App, Android]
---


![006](https://user-images.githubusercontent.com/72294509/156006719-2f81a767-3ec2-4d5e-bdbb-361ce393feaf.png)

<b>Hanssem AR(Augmented Reality) Placement application development</b>
<br>본 게시글은 한샘 AR 어플리케이션 개발 인턴 때 작성한 글입니다.
<br>(21/07/01 - 21/08/31)
<br><br>

# 1. ARCORE ?

 ARCORE은 AR 경험을 빌드하기 위한 Google의 플랫폼이며, 다양한 API를 사용하여 휴대폰에서 가상 contents를 볼 수 있다. 


> 💡 Android 7.0 (Nougat) 이상 Android 휴대폰에서 작동하도록 설계.

<br>

기본적으로 ARCORE는 두 가지 작업을 수행한다. 

1. 모바일 장치가 이동할 때 그 위치를 추적한다.
2. 실제 세계에 대한 지속적인 이해를 구축한다. 

 ARCORE의 동작 추적 기술은 Android 카메라를 사용하여 features(Interesting points)를 추출한다. 또한 해당 points들의 시간에 따른 움직임을 추적한다. 

 points들을 추출하는 것 뿐만 아닌, plain(평면) detection을 감지할 수 있고, 주변 영역의 평균 조명 값을 추정할 수 있다. <br>ARCORE는 이러한 모든 기능들이 결합되어, 카메라로 실제로 촬영되는 공간에 대한 자체 이해를 구축한다. 따라서 촬영되는 현실 세계에 대한 구축된 이해를 바탕으로 객체, 주석, 또는 기타 정보 등을 가상으로 배치할 수 있다. 

![Untitled](https://user-images.githubusercontent.com/72294509/156006761-58253395-d5c1-4c94-9620-26cadbf27da2.png)
<br>

# 2. Fundamental concepts

## 2-1. Motion tracking

 실제 환경을 촬영하고 있는 핸드폰이 움직일 때, ARCORE은 'simultaneous localization and mapping' (= SLAM) 이라는 process를 사용하여 주변 환경과 관련된 위치를 파악한다. <br>ARCORE는 카메라의 이미지에서 시각적으로 구분되는 특징인 'Feature points'를 감지하고 이를 통하여 위치의 변화를 감지한다.  <br>해당 points들은 IMU(관성 측정 장비)의 관성 측정치와 결합되며, 시간이 지남에 따라 전 세계를 기준으로 현재 카메라가 촬영하고 있는 장소의 'Pose' (Position and Orientation)를 추정하게 된다. 
<br><br>

> 💡 SLAM은 주로 자율 주행 차량에 사용되며, 기기의 주변 환경 지도를 작성하는 동시에, 기기의 위치를 작성된 지도 안에서 인식하는 기법이다.  이 알고리즘을 통해 기기는 미지의 환경에 대한 지도를 작성할 수 있으며, 엔지니어는 지도 정보를 활용하여 환경 파악 및 장애물 회피 등의 작업을 수행할 수 있다.

<br><br>

> 💡 SLAM process는 조금 더 광범위한 의미로 COM(concurrent odometry and mapping) process라고도 불린다.

<br><br>
 3D contents를 rendering하는 가상 카메라의 Pose와 ARCORE에서 제공하는 나의 핸드폰 장치 카메라의  Pose를 일치 시킨다. <br>따라서 개발자는 올바른 위치에 가상 3D contents를  rendering할 수 있으며, 핸드폰 장치에서 촬영되고 있는 화면과 rendering되는 가상 이미지가 겹쳐져 가상 contents가 실제 촬영되고 있는 세계의 일부인 것처럼 보이게 한다.

## 2-2. Environmental understanding

 ARCORE는 feature points와 plane(평면)을 감지함으로써 현실 세계에 대한 이해도를 지속적으로 높인다. 

 ARCORE는 테이블이나 벽과 같은 일반적인 수직이나 수평의 표면에 있는 것처럼 보이는 feature points의 cluster(군집들)를 찾고, 이러한 표면들을 APP 상에서 plane으로 사용할 수 있도록 한다. <br>ARCORE는 또한 각 평면의 경계를 결정하고, 해당 정보를 앱에 제공할 수 있다. 따라서 평면과, 평면의 경계 등의 여러 정보들을 사용하여 plane에 가상 개체를 배치할 수 있다. 

<br>

> 💡 ARCORE는 feature points(시각적으로 구분되는 점)들을 사용하여 plane을 감지하기 때문에, 질감과 물건이 없는 흰색 벽, 평면은 제대로 감지되지 않을 수 있다.

<br>

## 2-3. Depth understanding

 ARCORE는 지원되는 Android 장치의 기본 RGB 카메라를 사용하여 points들과 표면 사이의 거리에 대한 데이터를 포함하는 깊이 맵을 만들 수 있다. 깊이 맵에서 제공하는 정보를 사용하여 가상 contents를 관찰된 표면과 정확하게 맞닿게 하거나, 실제 개체의 앞이나 뒤에 표시되도록 하는 등, 사실적인 사용자 화면을 구현할 수 있다. 

[ARCore 깊이](https://codelabs.developers.google.com/codelabs/arcore-depth?hl=ko#0)
<br>

## 2-4. Light estimation

 ARCORE는 환경의 조명에 대한 정보를 감지하고, 주어진 카메라 이미지에 대한 평균 강도와 색상 보정을 제공할 수 있다. <br>해당 정보를 사용하여 주변 환경과 동일한 조건에서 가상 contents를 비추어 현실감을 높인다. 

## 2-5. User interaction

 ARCORE는 hit testing을 사용하여 휴대 전화 화면상의 (x, y) 좌표를 취하고, 카메라의 세계에서 광선을 투과하여 모든 평면과 광선이 교차하는 feature points, 세계 공간에서 교차하는 pose들을 활용하여 사용자는 환경에서 개체를 선택하거나 다른 방식으로 상호작용 할 수 있다. 
<br><br>

> 💡 Hit testing은 적중 테스트라고 하며, 포인트(터치 포인트 등)이 화면에 그려진 그래픽 객체(UIView 등)과 만나는지 여부를 결정하는 Process이다. <br>사용자가 기기 화면을 Tap하면 ARCORE는 해당 (x,y) 좌표에서 광선이 뻗어나간다 가정하고, 해당 광선이 교차하는 모든 plane, feature points, pose를 반환한다. 해당 결과로 Anchor를 만들게 된다.

<br>

```java
// Create a hit test using the Depth API.
List<HitResult> hitResultList = frame.hitTest(tap);
for (HitResult hit : hitResultList) {
    Trackable trackable = hit.getTrackable();
    if (trackable instanceof Plane
        || trackable instanceof Point
        || trackable instanceof DepthPoint) {
            doSomething(hit.createAnchor());
    }
}
```

→ Android App에서 Depth function 사용 시 Hit testing

## 2-6. Oriented points

 방향이 지정된 점을 사용하여 각진 표면에 가상 contents를 배치할 수 있다. <br>여러 points를 반환하는 hit testing을 수행할 때, ARCORE는 가까운 feature points를 보고, 이를 사용하여 주어진 feature points에서 표면의 각도를 추정한다. ARCORE는 해당 각도를 고려한 pose를 반환한다. 

```java
Pose objectPose = currentAnchor[0].getPose();
```

→ .getPose(); 사용하여 특정 Point의 Pose 반환 가능 

<br>

> 💡 ARCORE는 표면의 각도를 감지하기 위해 feature points를 사용하기 때문에, 흰색 벽과 같은 질감이 없는 표면은 제대로 감지되지 않을 수 있다.


## 2-7. Anchors and trackables

 ARCORE가 자신의 위치와 환경에 대한 이해를 향상 시키면서 pose가 변경될 수 있다. <br>가상 objects를 배치하려면 ARCORE가 시간이 경과됨에 따라 개체의 위치를 추적하도록 가상의 마커 역할을 하는 'Anchor'를 정의해야 한다. <br>보통 hit testing에서 반환된 pose를 기반으로 Anchor를 만들고 사용한다. 

 pose가 변경될 수 있다는 사실은, ARCORE가 시간이 지남에 따라 평면과 feature points와 같은 환경적 object들에 대한 위치 변경 정보를 업데이트 할 수 있다는 의미이다. <br>plane들과 points는 'trackable'라고 불리는 특수한 타입의 object들이다. 이름에서 알 수 있듯이 이는 ARCORE가 시간이 지남에 따라 추적할 개체들이다. 가상 object들을 'trackable' object들에 고정하여 휴대폰 장치가 움직일 때에도, 둘 사이의 관계는 안정적으로 유지되도록 할 수 있다. 
<br><br>

> 💡 Anchor는 CPU costs를 발생시키므로, 가능하면 재사용하며 더 이상 필요하지 않을 시 detach한다.

<br>

![Untitled 1](https://user-images.githubusercontent.com/72294509/156007322-3ef689d0-60d3-4478-8423-1a57fef47806.png)

```java
Anchor anchor = hitResult.createAnchor(); 
// plane tap시 hitresult 에서 anchor 생성
AnchorNode anchorNode = new AnchorNode(anchor); 
// 공간에 anchor을 기반으로 자동으로 배치되는 Node(Node : 하나의 오브젝트가 차지하는 영역)
anchorNode.setParent(arFragment.getArSceneView().getScene());
// (getScene : 장면 반환 / getArSceneView : 장면 랜더링(arsceneview) 반환) 
// -> parentNode로 set
```

## 2-8. Augmented Images

 Augmented Images는 특정 2D 이미지에 응답할 수 있는 AR app을 구축하는 기능이다. 사용자가 휴대 전화의 카메라로 특정 이미지를 촬영할 때, AR 경험을 트리거할 수 있다. 

 ARCORE는 흔들리는 2D 이미지 또한 추적할 수 있다. 

 이미지를 오프라인으로 컴파일하여 이미지 데이터베이스를 만들거나, 개별 이미지를 장치에서 실시간으로 추가할 수 있다. 해당 이미지가 등록될 때 마다, ARCORE는 이러한 이미지, 이미지의 경계들을 감지하고 해당 pose를 반환하게 된다. 

## 2-9. Sharing

 ARCORE Cloud Anchor API를 사용하여 Android 및 iOS장치 용 협업 또는 멀티 플레이어 앱을 만들 수 있다. 

 하나의 장치가 hosting을 위해 Anchor와 주변 feature points를 cloud로 전송한다. <br>
 이러한 Anchor는 동일한 환경의 Android 또는 iOS 장치에서 다른 사용자와 공유할 수 있다. 이를 통해 app은 이러한 Anchor에 연결된 동일한 3D object를 rendering하여 사용자가 동시에 동일한 AR 경험을 가질 수 있다.

## 2-10. ARCORE ELEMENTS

![Untitled 2](https://user-images.githubusercontent.com/72294509/156006741-2bbd1363-4db7-4e8d-b7f2-d56411c27786.png)
<br><br>

# 3. ARCORE + OPENGL

## 3-1. Open Graphics Library = OpenGL

3D 그래픽인 3차원 그래픽 응용프로그램을 만들기 위한 API이며, 여러 기능 지원.

→ 점, 선, 면 등과 같은 3차원 요소와 비트맵 등의 2차원 요소의 표현, 변환, 행렬을 통한 이들 요소의 변형

→ RGBA 모델과 INDEXED COLOR모델에 의한 색상 지원

→ 다양한 조명과 쉐이딩 설정

→ 텍스쳐 매핑

→ Antialiasing, Blending, Motion Blur... 등등

## 3-2. Why Not

- ARCORE를 사용할 때, ARCORE가 제공하는 기본적인 기능을 이용하여 feature point를 추출하고 point cloud를 구성, plane을 인식하기 까지의 과정을 직접 제어해주는 방법
- 각 데이터를 사용하기 위해 OpenGL을 이용한 rendering 과정이나 데이터 처리과정도 거쳐야 하므로 작업이 좀 까다로움
<br><br>

# 4. ARCORE + SCENEFORM

## 4-1. Sceneform

→ ARCORE에서 제공하는 3D Framework

→ High-level scene graph API

→ Filament에서 제공하는 Realistic physically based renderer

→ Android Studio plugin for importing, viewing, and building 3D assets.

<br>

> 💡 OpenGL 없이도 AR, 비 AR app에서 사실적인 3D 장면 rendering 가능

<br>

## 4-2. Sceneform SDK version 1.16.0

→ open source.

[google-ar/sceneform-android-sdk](https://github.com/google-ar/sceneform-android-sdk)

→ Build with application as a Gradle module.

→ **Supports glTF** instead of SFA and SFB Sceneform formats.
<br><br>

# 5. ARCORE METHODS

[5-1. Interfaces](https://www.notion.so/b5534da149c845ae95eebf2794228ed2)

[5-2. Classes (Image, Face 제외)](https://www.notion.so/6f38caaebc7f425ba9e50e43c8f87d6b)

[5-3. Enums](https://www.notion.so/cc0fee4f96fd4b70afab623d8bb23f67)
<br><br>

# 6. DEVELOP

- ARCORE + SCENEFORM 방식을 활용하여 Hanssem AR Furniture placement app을 개발하도록 한다.
- 3D objects → .glb file 형식
- 가구 선택 부분 및 모델 연결을 제외한 **'배치'** 기능과 **'길이 측정'** 기능 중점적으로 개발

## 6-1. Coding

fragment로 구현 된 프로젝트 

- fragment

→ FragmentActivity 내의 어떤 동작 또는 사용자 인터페이스의 일부<br>
→ Activity의 모듈식 세션<br>
→ AR 시스템 상태를 관리하고 세션 수명주기 처리 ( ARCORE API의 주요 진입 점 )<br>
→ 카메라 이미지와 장치 Pose에 접근할 수 있는 Frame을 수신할 수 있다.<br>

```java
private ArFragment arFragment;
private Renderable renderable;
```
<br>

- ArFragment

→ AR APP의 필수 요소, ARCORE의 기본 구성 요소들을 사용하게 해주는 SCENEFORM fragment.<br>

- Renderable

→ Node(하나의 Object가 차지하는 영역)에 연결하여 하나의 Object를 3D 공간에서 rendering 하기 위한 기본 Class<br>