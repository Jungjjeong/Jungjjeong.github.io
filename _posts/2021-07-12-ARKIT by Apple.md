---
layout: single
title: "[iOS] ARKIT by Apple"
categories: [App, iOS]
last_modified_at: 2021-07-12
excerpt: "ARKIT에 대해 알아보자"
header:
  teaser: https://user-images.githubusercontent.com/72294509/156119010-001d2fb2-be0f-4f08-83bf-3dd475785de9.png
---

![007](https://user-images.githubusercontent.com/72294509/156119010-001d2fb2-be0f-4f08-83bf-3dd475785de9.png)

<b>Hanssem AR(Augmented Reality) Placement application development</b>
<br>본 게시글은 한샘 AR 어플리케이션 개발 인턴 때 작성한 글입니다.
<br>(21/07/01 - 21/08/31)
<br><br>

# 1. ARKIT?

**iOS 기기를 위한 Apple의 AR 개발 플랫폼**

→ iOS 기기의 카메라와 움직임들을 어플 또는 게임 내에서 증강 현실을 구현하기 위해 통합하는 프레임워크

→ ARKIT는 기기의 움직임 추적, 카메라 장면 캡처 등 AR을 구현하기 위해 필요한 기술들을 결합해서 AR구현을 단순화하고, 이렇게 단순화 된 ARKIT 프래임워크를 사용해서 개발자들은 여러 종류의 AR을 기기의 정면, 후면 카메라를 이용해서 구현할 수 있을 것.

→ 2017년 6월 처음 출시

![Untitled](https://user-images.githubusercontent.com/72294509/156118973-8eca89a5-195e-4e29-8733-ffbe507cde36.png)

> 💡 ARKIT은 mac 내 Simulator로 구동 불가, ARKIT 사용 위해서는 iOS 11 이상의 기기와 Xcode 9 이상의 개발 환경 필요.

- Tutorial

[참고한 Tutorial](https://www.youtube.com/watch?v=f3xFpRWZEz8)

## 1-1. ARKIT algorithm

VIO(Visual-Inertial Odometry)라는 기술을 활용하여 스마트폰이 주변의 실제 환경에 대한 상대적인 위치를 이해한다.

`1) 모션 추적`

→ RGB 카메라 센서에서 오는 시각 데이터와 모션 데이터를 결합하여 accelerometer and gyroscope sensors 고대비의 위치 계산 (스마트폰의 IMU 센서)

→ 가상 카메라의 위치와 방향을 얻을 수 있다. (Feature points)

`2) 장면 이해`

→ 여러 Feature points이 동일 평면에 있는 위치를 이해하는데에 집중

→ 스마트폰이 감지하는 평면의 위치를 찾을 수 있음

`3) Rendering`

→ 가상의 물체를 고정된 anchor위에 배치

Arcore가 feature points의 확장에 조금 더 중점을 둔다면,

Arkit는 feature points들을 이용한 수직, 수평 → 평면 detection에 중점을 둔다.
<br><br>

# 2. ARKIT 기능

현재 ARKIT은 4까지 (2021/06) 출시된 상황

- ARKIT Framework Tree

[Apple Developer Documentation](https://developer.apple.com/documentation/arkit)

## 2-1. Depth API

아이폰 12 pro부터 탑재된 LIDAR 스캐너 내 고급 장면 인식 기능을 통해 주변 환경에 대한 픽셀당 Depth 정보를 사용할 수 있다.

> 💡 LIDAR : 사물에 레이저를 쏘고 빛이 반사되어 돌아오는 시간을 통해 사물까지 거리 값 (Depth) 측정하는 센서

해당 Depth 정보를 Scene Geometry에서 생성된 3D 매시 데이터와 결합하면 가상 3D Object를 즉각적으로 배치하고 실제 환경에 원활하게 혼합하여 가상의 사물 오클루전을 더 사실적으로 구현할 수 있다. 이를 통해 더 정밀한 측정 및 사용자 환경에 효과 적용 등 앱 내에서 새로운 기능을 구현할 수 있다.

> 💡 Occlusion (가림 효과) : 실제 사물 뒤에 가상 contents가 있을 때, 실제 사물에 의해 가상 contents가 가려지도록 하는 효과

## 2-2. Location Anchor

유명 랜드마크 및 도시의 곳곳 특정 위치에서 AR 경험을 구현할 수 있다. 위치 Anchor를 통해 특정 위도, 경도 및 고도 좌표에 AR 창작물을 고정할 수 있으며, 사용자는 카메라 렌즈를 통해 실제 물체를 보는 것처럼 가상 물체를 움직이고 다른 시각에서 확인할 수 있다.

## 2-3. 얼굴 추적 기능 지원 확대

**얼굴 추적 기능** (얼굴을 인식하고 ARKIT 상 좌표계 (x, y, z) 축 오버레이, 각 눈의 위치 및 방향 추적)에 대한 지원이 iPhone SE를 포함하여 A12 Bionic 칩 이상이 탑재된 모든 기기의 전면 카메라로 확대되어, 더 많은 사용자가 전면 카메라를 사용하여 AR 경험을 즐길 수 있다. TrueDepht 카메라를 사용하여 한번에 최대 3명의 얼굴을 추적하면서 미모티콘 및 Snapchat와 같은 전면 카메라 경험을 지원한다.

## 2-4. Scene Geometry

Scene Geometry로 바닥, 벽, 천장, 창문, 문, 좌석을 식별하는 레이블을 사용하여 공간의 토폴로지 맵을 만들 수 있다. 이처럼 실제 세계에 대한 심층적 이해를 통해 가상 물체에 대한 사물 occlusion 및 실제 물리적 요소를 구현하고 AR 작업 흐름을 구동하는 데 필요한 정보를 더 많이 얻을 수 있다.

## 2-5. LIDAR 스캐너 이용

LIDAR 스캐너를 사용하여 AR 물체를 스캔 없이 현실 세계에 즉각적으로 배치할 수 있다. 즉각적인 AR 배치 기능은 별도의 코드 변경 없이 iPhone 12 Pro, iPhone 12 Pro Max, iPad Pro에서 ARKIT으로 빌드한 모든 앱에 대해 자동으로 활성화 된다.

## 2-6. 인물 occlusion

AR 콘텐츠를 현실 세계에 있는 인물의 앞이나 뒤에 사실적으로 표시하면서 AR 경험을 더 몰입감 있게 만드는 동시에 대부분의 환경에서 '그린' 화면 스타일 효과를 적용할 수 있다. Depth 추정은 별도의 코드 변경 없이 iPhone 12 Pro, iPhone 12 Pro Max, iPad Pro에서 ARKIT으로 빌드한 모든 앱에 대해 자동으로 활성화 된다.

## 2-7. 모션 캡처

한 대의 카메라로 사람의 움직임을 실시간으로 캡쳐한다. 몸의 위치와 움직임을 일련의 관절과 뼈로 파악하여 AR 경험에 동작과 자세를 입력하면서 AR의 중심에 인물을 배치할 수 있다. 높이 추정은 별도의 코드 변경 없이, iPhone 12 Pro, iPhone 12 Pro Max, iPad Pro에서 ARKIT으로 빌드한 모든 앱에 대해 자동으로 활성화 된다.

## 2-8. 전 후면 카메라 동시 지원

전면 카메라, 후면 카메라로 얼굴과 공간을 동시에 추적할 수 있어 새로운 가능성이 열린다. 예를 들어 사용자는 자신의 얼굴만을 사용해 후면 카메라 뷰에 보이는 AR 콘텐츠와 상호 작용할 수 있다.

## 2-9. 다중 얼굴 추적

ARKIT 얼굴 추적 기능은 Apple Neural Engine과 전면 카메라가 탑재된 모든 기기에서 한번에 최대 3명의 얼굴을 추적하면서 미모티콘 및 Snapchat과 같은 AR 경험을 지원한다.

## 2-10. 협업 세션

여러 사람들과의 실시간 협업 세션을 사용하여 공동으로 AR 앱을 제작할 수 있으므로 AR 경험을 더욱 빠르게 개발하고 사용자들이 공유 AR 경험을 멀티 플레이어 게임처럼 즐기게 할 수 있다.

## 2-11. 추가 개선 사항

→ 한 번에 최대 100개의 이미지를 인식하고 해당 이미지에 있는 물체의 실제 크기를 자동으로 추정한다.

→ 복잡한 환경의 물체 인식이 개선되면서 3D 물체 인식이 더 강력해졌다.

→ 이제 머신 러닝을 사용하여 환경에서 더욱 빠르게 평면을 감지할 수 있다.
<br><br>

# 3. ARKIT or ARCORE?

→ ARKIT : 2017년 6월

→ ARCORE : 2018년 3월

BUT, ARCORE는 TANGO 플랫폼에서 발전한 플랫폼으로 ARKIT에 비해 크게 뒤쳐지지 않음

## 3-1. 지원 기능

![Untitled 1](https://user-images.githubusercontent.com/72294509/156119181-7136742e-d5c7-4db8-a4ff-1ecc69f76d05.png){: .align-center}

기본적으로 제공하는 기능은 거의 동일하나

ARKIT → 3D Object Tracking / Body Tracking, 전 후면 카메라 동시 지원, Multiple Face Tracking의 **추가적인 SDK 지원**

## 3-2. 범용성

- **ARCORE**

Android, iOS, Windows10에서 전부 구동 가능

Document → Unity, Unreal, Android, iOS 제공

Opensource

- **ARKIT**

iOS에서만 구동 가능하며 폐쇄적으로 Document, Opensource 제공 X

## 3-3. 인식 기능 (Algorithm 방식)

- **ARCORE**

Feature Points를 최대한 많이 찾아, Mapping된 공간 영역을 빠르게 확장

- **ARKIT**

수평, 수직으로 평면을 감지하는 기술이 정확

## 3-4. 지원 디바이스

- **ARCORE**

Google/ 하웨이/ LG/ 삼성/ 샤오미/ Apple 등 폭넓은 디바이스 지원

→ 하드웨어 성능, 지원하는 센서에 따라 ARCORE 성능이 많이 달라짐

- **ARKIT**

iPhone / iPad 의 iOS 기기만 지원

→ True Depth 카메라가 장착되어 있어 정확도가 상당히 높은 인식 기술 제공

→ 특히 LIDAR 부착된 장치의 경우, 고해상도의 Depth Map을 얻을 수 있어 AR 세계의 공간을 분석하고 인식하는 기술이 더욱 향상됨.
<br><br>

# 4. CLASS

## 4-1. ARConfiguration

→ AR에 필요한 것들을 구성하기 위한 추상 Class.

→ 이를 상속받은 8개의 서브클래스가 실질적인 작업을 한다.

![Untitled 2](https://user-images.githubusercontent.com/72294509/156119205-06624c29-21b8-4584-8417-19f389a1bdea.png){: .align-center}

→ ARWorldTrackingConfiguration

ARkit 기본 기능 제공, 사용자가 거주하는 실제 세계를 추적하고 가상 콘텐츠를 배치할 좌표 공간과 일치시킴

가상의 물체는 현실 세계의 카메라가 어떻게 움직이더라도 같은 곳에 위치해야 한다.

![Untitled 3](https://user-images.githubusercontent.com/72294509/156119215-5427f25a-9adb-44a6-a783-4f7d28eab0e1.png){: .align-center}

기기의 움직임을 3개의 회전각(roll, pitch, yaw)과 3개의 변환각 (x, y, z)을 통해 추적한다.
<br><br>

# 5. SCENEKIT

Scenekit는 앱에서 3D 애니메이션 장면과 효과를 만드는 데 도움이 되는 고급 3D 그래픽 프래임워크.

![Untitled 4](https://user-images.githubusercontent.com/72294509/156119230-13eb3104-ecf8-4cab-ada1-fb187f350cd5.png){: .align-center}

> 모바일 환경에서 증강현실 혹은 3D 모델 랜더링을 요하는 어플리케이션을 개발할 때, OpenGL이나 Apple의 Metal과 같은 그래픽 API들을 표준으로 삼음.

> 하지만 개발 시 매우 어려운 난이도나, 비효율성 등의 여러 문제 때문에 graphics rendering engine을 필요로 함 → **SpriteKit, SceneKit**

## 5-1. SpriteKit

→ 단순 규모에서 2D 애니메이션을 위해 디자인 됨.

→ 개발자들에게 2D 애니메이션을 위한 모든 툴 제공.

## 5-2. SceneKit

→ Core Animation Framework를 기반으로, SpriteKit에 비해 고성능 옵션을 제공하며, 사용시 상당한 수학과 기하학 요구

→ high-level 3D graphics framework로서, rendering engine과 descriptive API로 움직이는 장면을 만들기 위해 사용된다.

→ Metal과 같은 lower-level-API보다는 복잡하다.

→ Loin OS에서 배포되었으며, 개발자들이 보다 쉽게 복잡한 3D 장면을 만들 수 있도록 디자인 됨.

→ VR 용으로 고안되엇으며 ARKIT과 함께 사용할 수 밖에 없음.

```swift
@IBOutlet weak var sceneView: SCNView!

sceneView.scene = SCNScene()
sceneView.autoenablesDefaultLighting = true
let boxNode = SCNNode()
boxNode.geometry = SCNBox(width: 0.5, height: 0.5, length: 0.5, chamferRadius: 0)
boxNode.geometry?.firstMaterial?.lightingModel = .physicallyBased
boxNode.geometry?.firstMaterial?.diffuse.contents = UIColor.red
boxNode.geometry?.firstMaterial?.metalness.contents = 1.0

sceneView.scene?.rootNode.addChildNode(boxNode)
```

→ 기본 View는 SCNView

- 보통 AR 증강현실 APP에서는 SceneKit를 사용해 왔음.

→ SceneKit은 Contents들을 트리구조를 활용해서 실행.

→ root 노드는 시각적인 요소를 가지지 않으며, 해당 Scene world의 좌표를 정함.

→ 자식 노드들은 root 노드의 scene world 내에서 시각적인 요소로 존재하는 원리.

> 💡 Node (SCNNode) : root노드에 상대적인 좌표 위치 변환 속성을 제공함 (position, orientation, scale)

→ 보는 방향이 -z축 방향을 가리키는 우회 좌표계를 사용한다.

![Untitled 5](https://user-images.githubusercontent.com/72294509/156118868-6bcc39c7-f835-4f69-9da7-5a398dc76792.png){: .align-center}

**SCENEKIT은 2017년 이후로 더 이상 업데이트 되지 않음**

→ 점점 사라질 예정

→ 하지만 아직도 RealityKit 2.0보다 몇몇 이점이 존재한다.
<br><br>

# 6. RealityKit 2.0

기본 ARKit 통합, 물리적 요소 기반의 랜더링, 변형 및 스켈레톤 애니메이션, 공간 오디오 및 강체 물리 요소를 통해 AR 개발을 빠르고 쉽게 수행할 수 있게 해주는 프레임워크.

![Untitled 6](https://user-images.githubusercontent.com/72294509/156118958-2372aa96-4eb1-4f45-b19a-bb408411f019.png){: .align-center}

→ Apple의 랜더링 기술 제품군에서 가장 최신 SDK (2019년 출시)

→ AR/ VR 프로젝트 용으로 제작되었으며, iOS, mac에서 사용 가능

→ 다중 스레드 렌더링

→ LIDAR 스캐너 지원

→ 독립적으로 사용 가능하며, ARKIT, MetalKit와 함께 사용할 수 있다.

→ iPhone OS 15에서부터 접근 가능

```swift
@IBOutlet weak var arView: ARView!
let box = MeshResource.generateBox(size: 0.5)
let material = PhysicallyBasedMaterial()
let model = ModelEntity(mesh: box, materials: [material])

let anchor = AnchorEntity(world: [0, 0,-1])
anchor.addChild(model)

arView.scene.anchors.append(anchor)
```

→ 기본 View는 ARView
