---
layout: post
title:  "[WWDC17] Introducing Drag and Drop(3/4)"
date:   2017-07-12 10:59:00 +0900
categories: wwdc wwdc17 
---

마지막으로 UIDropInteractionDelegate 에 대해 알아보겠습니다.

[https://developer.apple.com/documentation/uikit/uidropinteractiondelegate](https://developer.apple.com/documentation/uikit/uidropinteractiondelegate)

---

# 드랍하기
``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, performDrop session: UIDropSession)
```
드랍 세션으로부터 데이터를 가져와서 드랍을 수행합니다.

``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, canHandle session: UIDropSession) -> Bool
```
드랍을 진행하기 전에 드랍을 계속 진행할건지 결정합니다.

``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, concludeDrop session: UIDropSession)
```
드랍에 대한 데이터 이동이 끝나고 모든 드랍 애니메이션이 종료된 후 호출됩니다.

---

# 드랍 애니메이션
``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, item: UIDragItem, willAnimateDropWith animator: UIDragAnimating)
```
커스텀 드랍 애니메이션을 만듭니다.

---

# 드랍 트랙킹
``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, sessionDidEnter session: UIDropSession)
```
드래그한 아이템이 드랍 인터랙션 뷰 안으로 들어와서 드랍 세션이 시작되었음을 알려줍니다.

``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, sessionDidUpdate session: UIDropSession) -> UIDropProposal 
```
드래그한 아이템이 드랍 인터랙션 뷰 안으로 들어왔을 때, 드랍 인터랙션 뷰 안에서 이동할 때, 드랍 인터랙션 뷰 안에서 드래그 아이템이 추가되었을 때 호출되며 리턴 값으로 UIDropProposal 값을 주어야 합니다.

UIDropProposal 은 앞선 글에서 설명했듯이 .cancel, .copy, .move, .forbidden 중에서 하나를 선택해야 합니다.

``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, sessionDidExit session: UIDropSession)
```
드랍 인터랙션 뷰로부터 드래그 아이템이 빠져나갔을 때 호출됩니다.

``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, sessionDidEnd session: UIDropSession)
```
드랍 세션이 완전히 끝났습니다. 이제 관련된 모든 임시 데이터들을 해제해도 됩니다.

---

# 프리뷰 만들기
``` swift
optional func dropInteraction(_ interaction: UIDropInteraction, previewForDropping item: UIDragItem, withDefault defaultPreview: UITargetedDragPreview) -> UITargetedDragPreview? 
```
드랍이 될 때 프리뷰 이미지를 따로 만들어줄 수 있습니다.

---

WWDC 세션 발표 내용 기준으로 다시 한 번 살펴보겠습니다.

\
아래 두 개의 예제는 드랍을 수행할지 결정하는 `dropInteraction(_:canHandle:)` delegate 를 사용하는 방법을 보여줍니다. 드랍을 수행하기 전에 내가 다룰 수 있는 데이터가 있는지 확인하고 있습니다.

![](/assets/170710/203_introducing_drag_and_drop24.jpg)

![](/assets/170710/203_introducing_drag_and_drop25.jpg)

![](/assets/170710/203_introducing_drag_and_drop26.jpg)

> 드래그 된 아이템이 드랍 인터랙션 뷰 안에 들어와 움직이는 모습을 캐치해서 처리해줍니다.

![](/assets/170710/203_introducing_drag_and_drop27.jpg)

버튼에 스프링로딩을 사용하는 방법입니다. 버튼에 `UISpringLoadedInteraction` 을 추가해 줍니다. `UISpringLoadedInteraction` 을 초기화할 때 해당 스프링로딩에 대한 액션을 지정해줄 수 있습니다.

![](/assets/170710/203_introducing_drag_and_drop28.jpg)

> 드랍 세션이 완전히 끝났습니다.

![](/assets/170710/203_introducing_drag_and_drop29.jpg)

> 드래그하던 아이템을 드랍 인터랙션 뷰 위에 떨어뜨려 최종적으로 드랍을 수행하는 모습입니다. 드랍 애니메이션과 데이터를 받는 과정을 동시에 처리합니다.

![](/assets/170710/203_introducing_drag_and_drop30.jpg)

> 드랍을 위한 새로운 프리뷰를 만들 수도 있고, 애니메이션에도 변화를 줄 수 있습니다.

![](/assets/170710/203_introducing_drag_and_drop31.jpg)

ItemProvider의 loadObject 는 데이터를 받는 동시에 progress 를 리턴해주기 때문에 아이템별로 진행 상태를 알 수 있습니다.

fractionCompleted: Double 를 통해 진행 상황을 알아냅니다.

isFinished 를 통해 모든 progress 가 끝났는지 알 수 있습니다.

cancel() 을 통해 데이터 받아오기를 취소할 수도 있습니다.

\
전체 세션에 대한 progress 도 session.progress 를 통해 알 수 있습니다.

\
이제 드래그 앤 드랍에 대한 기초적인 내용을 모두 살펴보았습니다.

\
드래그 앤 드랍에 대한 관련 세션으로는 다음과 같은 내용이 더 있습니다.


- Mastering Drag and Drop
- Data Delivery with Drag and Drop
- Drag and Drop with Collection and Table View 
- File Provider Enhancements

\
애플에서는 드래그 앤 드랍을 위한 커스터마이징 포인트를 많이 제공하고 있어서 언뜻 보기에 복잡해 보일수도 있지만, 핵심 기능만 보면 매우 간단하게 드래그 앤 드랍을 구현할 수 있습니다.