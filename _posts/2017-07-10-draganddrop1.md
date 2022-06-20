---
layout: post
title:  "[WWDC17] Introducing Drag and Drop(1/4)"
date:   2017-07-10 11:19:00 +0900
categories: wwdc wwdc17 
---
iOS 11 api의 하이라이트는 드래그 앤 드랍입니다.

PC에서는 당연한 기능이고, 안드로이드에서도 일부 지원하고 있었죠.

iOS는 어떻게 보면 좀 늦었다고도 볼 수 있는데, 실제 작동하는 모습을 보면 손바닥에서 돌아가는 모바일 OS 에서의 드래그 앤 드랍이 과연 필요할까에서부터 어떻게 해야 환상적인 UX 로 구현할 수 있을까를 오랜 시간에 걸쳐 절절이 고민한 흔적이 보입니다.

\
드래그 앤 드랍을 여러분의 앱에 어떻게 추가할 수 있을지 알아보도록 하겠습니다.

![](/assets/170710/203_introducing_drag_and_drop.jpg)

드래그 앤 드랍이란 무엇일까요?

사용자가 이용하는 포인팅 장치를 이용해 데이터 이동 또는 복사를 직관적으로 할 수 있는 기능을 말합니다.

PC에서는 마우스를 사용하지만, 마우스의 포인트는 단 하나죠.

iOS에서는 멀티 터치를 사용해 PC에서보다 더욱 편리하게 드래그 앤 드랍을 사용할 수 있게 되었습니다.

데이터는 드랍되는 목적지에서만 접근 가능하며, 비동기로 로드할 수 있습니다.

\
드래그 앤 드랍은 iOS의 기능이기 때문에 아이패드와 아이폰에서 동일하게 동작합니다. 단, 아이폰은 두 가지 앱의 UI가 동시에 표시되는 멀티태스킹 기능이 없고 화면이 작기 때문에, 앱 간의 데이터 이동은 불가능합니다.

![](/assets/170710/203_introducing_drag_and_drop2.jpg)

드래그 앤 드랍은 다음과 같은 순서에 의해 작동됩니다.

1. Lift : 드래그 할 뷰를 롱프레스 하는 걸로 시작합니다. 드래그가 시작되면 해당 뷰가 붕 떠오르는 애니메이션으로 드래그 앤 드랍이 시작되었음을 유저에게 알려줍니다.
2. Drag : 이제 Lift된 뷰를 이리저리 움직일 수 있고 목적지로 옮길 수 있습니다. 이 때 데이터의 Preview 화면이 화면을 따라다닙니다. 멀티터치를 이용해 드래그가 가능한 다른 뷰를 터치하면 해당 뷰가 드래그 세션이 추가됩니다. Spring-Loading(아이콘이나 폴더)이 가능한 뷰로 손가락을 옮기면 Spring-Loading 이 이루어집니다.
3. Set Down : 드래그 되고 있는 손가락을 화면으로부터 띠었을 때 발생합니다. 해당 위치에서 드랍을 받아줄 수 없다면 그대로 드래그 세션은 취소됩니다. 드랍을 받아줄 수 있는 뷰라면 드랍을 진행하게 됩니다.
4. Data Transfer : 드랍을 진행하게 되며, 해당 데이터를 받아옵니다. 드랍이 된다는 애니메이션이 표시되며, 이 때 데이터는 비동기로 받아올 수 있습니다.

![](/assets/170710/203_introducing_drag_and_drop3.jpg)

드래그를 시작하기 위해선 UIDragInteraction 을 view 에 지정해주면 됩니다.

UIDragInteraction 을 만들기 위해선 UIDragInteractionDelegate 가 필요한데요, 이 delegate 의 필수 메소드는 어떤 드래그 아이템을 지정할 것인지 정하게 됩니다.

``` swift
func dragInteraction(_ interaction: UIDragInteraction, itemsForBeginning session: UIDragSession) -> [UIDragItem]
```

이 delegate 메소드에서 적절한 UIDragItem 들을 리턴해주면, 드래그의 Lift 동작이 시작되고, 빈 array 를 리턴하면 드래그가 시작되지 않습니다.

![](/assets/170710/203_introducing_drag_and_drop4.jpg)

UIDragItem 는 어떤 데이터를 가지고 있는지 알고 있는 Item Provider(NSItemProvider) 와 드래그 시 보여줄 preview 정보를 가지고 있습니다.

이제 Drop 을 수행하기 위한 가장 간단한 방법을 알아보겠습니다.

UIDropInteraction 이 있지만, 이걸 사용하지 않고 Paste Configuration 을 사용할 수 있습니다.

![](/assets/170710/203_introducing_drag_and_drop5.jpg)

Paste Configuration 은 UIResponder 의 속성이며, Copy-and-Paste 를 수행할 때, 어떤 데이터를 받아줄 수 있는지 설정하게 됩니다.

위와 같이 View에 Paste 설정을 하게 되면

``` swift
override func paste(itemProviders: [NSItemProvider]) {
}
```

Paste 또는 Drop 액션이 발생될 때 paste(:) 메소드가 호출되고 데이터를 받아줄 수 있습니다.

여기까지의 코드가 궁금하시면 다음을 참고하세요

[https://github.com/ricky-choi/Drag-and-Drop-Tutorial/tree/Step2_SimpleDrag](https://github.com/ricky-choi/Drag-and-Drop-Tutorial/tree/Step2_SimpleDrag)

Paste Configufation 을 이용한 방법은 사실 Drop 액션이 아니라 말 그대로 붙여넣기가 가능하기 때문에 Drop 도 가능한 케이스였습니다.

이제 Paste Configuration 이 아닌 UIDropInteraction 을 이용해 진짜 Drop 액션을 만들어보겠습니다.