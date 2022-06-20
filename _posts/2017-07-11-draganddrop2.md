---
layout: post
title:  "[WWDC17] Introducing Drag and Drop(2/4)"
date:   2017-07-11 09:59:00 +0900
categories: wwdc wwdc17 
---

이제 Paste Configuration 이 아닌 UIDropInteraction 을 이용하는 방법을 알아보겠습니다.

![](/assets/170710/203_introducing_drag_and_drop6.jpg)

Drop 을 하기 위한 초기화는 Drag 와 완벽히 동일합니다.

UIDropInteractionDelegate 를 가지는 UIDropInteraction 을 View 에 추가시켜줍니다.

실제 중요한 작업은 UIDropInteractionDelegate 에서 처리하게 됩니다.

![](/assets/170710/203_introducing_drag_and_drop7.jpg)

![](/assets/170710/203_introducing_drag_and_drop8.jpg)

지금까지의 내용을 정리한 Dran and Drop API Roadmap 입니다.

파란색은 드래그에 대한 부분입니다.

회색은 드래그 되는 아이템에 대한 부분입니다.

녹색은 드랍에 대한 부분입니다.

\
이제 이 내용들에 대해 더 상세히 알아보겠습니다.

드래그 앤 드랍이 이뤄지는 모습을 타임라인으로 표현한 모습입니다.

![](/assets/170710/203_introducing_drag_and_drop9.jpg)

> 드랍 포지션에 받아줄 뷰가 없을 경우 드래그 앤 드랍 액션은 취소됩니다.

![](/assets/170710/203_introducing_drag_and_drop10.jpg)

> 드랍 포지션에서 드랍을 받아주면 드랍 애니메이션과 함께 해당 아이템의 데이터를 받게 됩니다.

이제 타임라인을 기준으로 드래그 앤 드랍의 3단계에 대해 설명하겠습니다.

타임라인에서 실선으로 표시된 부분이 기준이며,

위에 설명한 UIDragInteraction 과 UIDropInteraction 을 설정해 준 상태에서 Delegate 구현부를 중심으로 보게 됩니다.

---

# Drag and Drop API Essential - 1/3 : 드래그할 아이템 얻기

![](/assets/170710/203_introducing_drag_and_drop11.jpg)

[https://developer.apple.com/documentation/uikit/uidraginteractiondelegate/2891010-draginteraction](https://developer.apple.com/documentation/uikit/uidraginteractiondelegate/2891010-draginteraction)

`dragInteraction(_:itemsForBeginning:)` 델리게이트를 통해 구현합니다.

\
드래그 하게 될 아이템을 리턴합니다.

NSItemProvider 를 포함하는 UIDragItem 의 배열입니다.

NSItemProvider 는 주로 NSItemProviderWriting 프로토콜을 준수하는 객체로 만들며, NSString, NSAttributedString, NSURL, UIColor, UIImage 등이 기본적으로 지원합니다.

# Drag and Drop API Essential - 2/3 : 드랍 가능 여부 결정

![](/assets/170710/203_introducing_drag_and_drop12.jpg)

드래그 하는 도중 preview 에 드랍 상태를 표시해 줄 수 있으며, 표시한대로 액션이 이루어지게 됩니다.
- cancel : 드랍할 수 없는 뷰입니다. 손가락을 띠면 그대로 드랍 액션이 취소됩니다.
- copy : 아이템을 복사합니다. 다른 앱에서 드래그 할 때는 반드시 복사만 가능합니다.
- move : 아이템을 이동합니다.
- forbidden : 일시적으로 드랍이 불가능함을 알리는 메시지입니다. 드랍이 가능한 폴더를 생각해봅시다. 여러 개의 폴더 중 한 폴더가 read-only 속성이 켜졌다면 해당 폴더 위에선 forbidden 마크가 표시될 겁니다.

![](/assets/170710/203_introducing_drag_and_drop13.jpg)

[https://developer.apple.com/documentation/uikit/uidropinteractiondelegate/2890888-dropinteraction](https://developer.apple.com/documentation/uikit/uidropinteractiondelegate/2890888-dropinteraction)

이 액션들은 dropInteraction(_:sessionDidUpdate:) -> UIDropProposal 을 통해 반영할 수 있습니다.

경우에 따라 위 네 개 속성 중 하나를 리턴해 줍니다.

# Drag and Drop API Essential - 3/3 : 드랍 수행하기

마지막으로 하게 될 드랍 액션은 dropInteraction(_:performDrop:) 을 통해 하게 됩니다.

아래 두 가지 방법이 나와 있습니다.

![](/assets/170710/203_introducing_drag_and_drop14.jpg)

첫번째는 UIDropSession 의 loadObjects(ofClass:) 를 사용하는 방법입니다.

세션에서 로드할 데이터의 종류를 표시해주면 데이터가 준비되었을 때 closure 를 통해 해당 데이터들을 한 번에 제공해줍니다. 이 때 리턴되는 closure 는 메인큐란 걸 기억해두세요.

![](/assets/170710/203_introducing_drag_and_drop15.jpg)

두번째는 조금 더 복잡하고 low level 을 이용하는 방법입니다. 세션에 속해 있는 아이템들(드래그로 선택한 아이템들)의 item provider(NSItemProvider) 에 직접 접근하는 방법입니다.

이 때에는 백그라운드큐이므로 리턴된 데이터를 UI 에 바로 반영하기 위해서 DispatchQueue.main.async 를 사용해야 한다는 걸 기억해 두세요.

![](/assets/170710/203_introducing_drag_and_drop16.jpg)

이제 드래그 앤 드랍의 주요 API 에 대한 정리를 다시 해보겠습니다.
1. 드래그 할 아이템들을 선택합니다.
2. 드래그한 아이템들을 드랍할 수 있을지 판단합니다.
3. 실제로 드랍한 아이템들의 데이터를 가져와서 처리해줍니다.

API 로 정리해보면
1.  dragInteraction(_:itemsForBeginning:)
2.  dropInteraction(_:sessionDidUpdate:) -> UIDropProposal
3.  dropInteraction(_:performDrop:)

지금까지의 내용은 다음 소스에 구현되어 있으니 참고하세요.

[https://github.com/ricky-choi/Drag-and-Drop-Tutorial](https://github.com/ricky-choi/Drag-and-Drop-Tutorial)