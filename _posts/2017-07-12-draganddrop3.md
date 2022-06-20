---
layout: post
title:  "[WWDC17] Introducing Drag and Drop(3/4)"
date:   2017-07-12 10:59:00 +0900
categories: wwdc wwdc17 
---
Drag Interaction 에 대한 상세 설정에 대해 알아보겠습니다.

\
먼저 UIDragInteractionDelegate 에 어떤 메소드들이 있는지 보죠. (이 글은 iOS 11 Beta 3 기준으로 작성되었습니다.)

[https://developer.apple.com/documentation/uikit/uidraginteractiondelegate](https://developer.apple.com/documentation/uikit/uidraginteractiondelegate)

---

# 드래그하기

``` swift 
func dragInteraction(_ interaction: UIDragInteraction, itemsForBeginning session: UIDragSession) -> [UIDragItem] Required
```
드래그 할 아이템을 리턴합니다. 리턴되는 array 가 비어 있으면 드래그를 시작하지 않습니다.

``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, itemsForAddingTo session: UIDragSession, withTouchAt point: CGPoint) -> [UIDragItem]
```
멀티터치를 이용해 드래그 아이템을 추가해줍니다.

``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionForAddingItems sessions: [UIDragSession], withTouchAt point: CGPoint) -> UIDragSession?
```
드래그 아이템을 추가할 때 드래그 세션이 하나 이상이라면 특정 드래그 세션을 지정해줍니다.

---

# 드래그 애니메이션

``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, willAnimateLiftWith animator: UIDragAnimating, session: UIDragSession)
```
드래그 시작 애니메이션을 정의합니다. animator:UIDragAnimating 을 사용해서 lift 애니메이션을 만들어줍니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, item: UIDragItem, willAnimateCancelWith animator: UIDragAnimating)
```
드래그가 취소될 때 애니메이션을 정의합니다.

---

# 드래그 상태 모니터링

``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionWillBegin session: UIDragSession)
```
드래그 시작 애니메이션이 끝나고 유저가 드래그를 시작했음을 알려줍니다. 이걸 이용해서 드래그 모드가 시작됐다는 걸 알 수 있습니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, session: UIDragSession, willAdd items: [UIDragItem], for addingInteraction: UIDragInteraction)
```
드래그 세션에 새 드래그 아이템이 추가됨을 알려줍니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionDidMove session: UIDragSession)
```

유저가 드래그 하고 있습니다. session:UIDragSession의 locationInView: 를 이용해서 위치를 가져올 수 있습니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, session: UIDragSession, willEndWith operation: UIDropOperation)
```
이제 곧 드래그 인터랙션이 끝납니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, session: UIDragSession, didEndWith operation: UIDropOperation)
```
유저가 드래그하던 아이템으로부터 손을 완전히 띠었습니다. 이제 드래그 인터랙션이 종료되었습니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionDidTransferItems session: UIDragSession)
```
드랍 위치에서 데이터를 완전히 받았습니다. 이제 완전히 끝났네요. 드래그를 위해서 임시로 보관하고 있던 데이터가 있었다면 이제 지워줍니다.

---

# 드래그 프리뷰 만들기
``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, previewForLifting item: UIDragItem, session: UIDragSession) -> UITargetedDragPreview?
```
lift 애니메이션을 위한 프리뷰를 만들어줍니다.

프리뷰는 UITargetedDragPreview 입니다.

이 메소드를 구현하지 않으면 기본으로 프리뷰를 제공하며, 드래그 인터랙션을 초기화한 View 가 그대로 보여지게 됩니다.

nil 을 리턴하면 lift 애니메이션 자체가 없어집니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, previewForCancelling item: UIDragItem, withDefault defaultPreview: UITargetedDragPreview) -> UITargetedDragPreview?
```
드래그가 취소될 때 프리뷰를 따로 만들어줄 수 있습니다. 아이템을 휴지 이미지로 만들어준다던가 할 수 있겠네요.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, prefersFullSizePreviewsFor session: UIDragSession) -> Bool
```
프리뷰의 원래 사이즈를 유지할건지 결정합니다.

---

# 드래그 제한하기
``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionIsRestrictedToDraggingApplication session: UIDragSession) -> Bool
```
true를 리턴하면 이 드래그는 현재 앱에서만 사용할 수 있습니다. 다른 앱으로 드래그할 수 없습니다.

false를 리턴하면 현재 앱과 다른 앱으로 모두 드래그할 수 있습니다.

기본값은 false 입니다.

UIDragDropSession의  var isRestrictedToDraggingApplication: Bool { get } 을 대신 사용해도 됩니다.


``` swift
optional func dragInteraction(_ interaction: UIDragInteraction, sessionAllowsMoveOperation session: UIDragSession) -> Bool
```
인앱 드래그를 할 때 move 가능 여부를 결정합니다.

---

# 아래 부분은 WWDC17 세션에서 소개한 내용입니다.

![](/assets/170710/203_introducing_drag_and_drop17.jpg)

> 드래그 프리뷰를 만드는 간단한 방법입니다.

![](/assets/170710/203_introducing_drag_and_drop18.jpg)

> UIDragAnimating 을 이용해 드래그 시작 애니메이션을 만들어줄 수 있습니다.

![](/assets/170710/203_introducing_drag_and_drop19.jpg)

> 드래그 세션이 시작되고 이동이 일어날 때 이런 순서로 delegate 가 호출됩니다.

![](/assets/170710/203_introducing_drag_and_drop20.jpg)

> 드래그 중 새로운 아이템을 드래그 아이템으로 선택했을 때 불리게 되는 delegate 메소드들입니다.

![](/assets/170710/203_introducing_drag_and_drop21.jpg)

> 드래그 세션이 곧 끝남을 알려줍니다.

![](/assets/170710/203_introducing_drag_and_drop22.jpg)

> 드래그가 취소되며 끝나면 불리게 됩니다.

![](/assets/170710/203_introducing_drag_and_drop23.jpg)

> 드래그가 정상적으로 드랍 액션과 함께 끝났을 때 불리게 됩니다.
