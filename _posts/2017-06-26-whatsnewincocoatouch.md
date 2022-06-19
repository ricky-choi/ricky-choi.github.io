---
layout: post
title:  "[WWDC17] What's New in Cocoa Touch"
date:   2017-06-26 14:42:00 +0900
categories: wwdc wwdc17 
---
# Drag and Drop

iOS 11 의 신기능 중 가장 눈에 띄는 것은 드래그 앤 드랍 기능입니다.

이제 PC 에서와 같이 드래그 앤 드랍을 통해 보다 편리한 사용이 가능해졌습니다.

iOS 만을 위한 새로운 UX 와 더해져 오히려 PC에서의 사용성보다 놀랍도록 뛰어난 부분도 있습니다.

\
드래그 앤 드랍을 위한 기본적은 사용법은 매우 쉬우며, 커스터마이징 기능 또한 완벽히 갖춰져 있습니다.

![](/assets/170626/201_whats_new_in_cocoa_touch.jpg)

드래그를 시작하는 주체의 셋팅은 위 그림과 같이 하면 됩니다.

UIDragIteractionDelegate 를 가지는 UIDragInteraction 을 만든 뒤 해당 오브젝트를 UIView에 추가합니다.

상세한 설정은 delegate 에서 수행하며 되며, 드래그 아이템의 데이터 제공, 드래그 시작 시의 애니메이션 커스터마이징, 드래그 아이템의 프리뷰 설정 등을 할 수 있습니다.

![](/assets/170626/201_whats_new_in_cocoa_touch2.jpg)

드래그 된 아이템을 받아줄 수 있는 드랍 주체에 대한 셋팅은 위 그림과 같이 합니다.

드래그 시와 완벽하게 동일합니다.

UIDropInteractionDelegate 를 가지는 UIDropInteraction을 만든 뒤 해당 오브젝트를 UIView 에 추가해 줍니다.

드랍 delegate 에서는 드래그 되어 온 아이템이 이동되어져 올 때의 UI 업데이트, 드랍 된 데이터를 받아주기, 드랍 애니메이션의 커스터마이징 등을 수행합니다.

\
드래그 앤 드랍은 다음 세션에서 더 상세한 내용을 볼 수 있습니다.
- Introducing Drag and Drop
- Mastering Drag and Drop
- Drag and Drop with Collection and Table View
- Data Delivery with Drag and Drop



# File Management

![](/assets/170626/201_whats_new_in_cocoa_touch4.jpg)

지난해동안 개발자들의 가장 많은 요청이 있던 기능 중 하나는 파일 브라우징 기능이었습니다.

iCloud 의 도입으로 클라우드 파일 스토리지의 사용이 가능하게 되었지만, iCloud 에서 파일 관리를 완벽하게 구현하는 건 어려운 일이었습니다.

이제 iOS 11 의 새로운 앱인 Files 와 유사한 파일 브라우징 뷰를 제공합니다.

UIDocumentBrowserViewController 를 이용하며, 자신의 앱에서 다룰 수 있는 파일 포맷에 대한 구현부만 작성하면 됩니다. 물론 배경화면이나 메인 컬러 등을 변경할 수 있고, 새로운 액션을 위한 버튼 등을 추가할 수도 있습니다.

UIDocument 클래스와 아주 유연하게 작동합니다.

\
자세한 내용은 다음 세션에 이어집니다.
- Building Great Document-Based Apps in iOS 11


# UI Refinements

![](/assets/170626/201_whats_new_in_cocoa_touch5.jpg)

iOS 11 의 가장 큰 특징이라면 바로 저 커다랗게 변한 타이틀 텍스트입니다.

기존에 비해 매우 커지고 눈에 띠게 만들어졌습니다.

또한 이 타이틀 아래에 검색 컨트롤러를 다는 일도 쉬어졌습니다.

스크롤뷰가 위로 스크롤 될 때에는 검색바와 타이틀바 영역이 기존처럼 다시 작아지기 때문에, 컨텐츠를 더욱 잘 보여주는 일도 여전히 잘하게 됩니다.

``` swift
class UINavigationBar {
  var prefersLargeTitle: Bool
}

class UINavigationItem {
  var largeTitleDisplayMode: LargeTitleDisplayMode // .always, .automatic, .never
}

class UINavigationItem {
  var searchController: UISearchController?
}
```

prefersLargeTitle 은 해당 Navigation Bar 가 큰 타이틀 뷰를 표시할 수 있음을 설정합니다.

prefersLargeTitle 을 설정할 경우 Navigation Bar 는 자동으로 Scroll View 를 찾아서 Scroll View의 Offset 를 트랙킹하며 네비게이션바의 확대와 축소를 하게 됩니다.

\
largeTitleDisplayMode 은 해당 View Controller 의 네비게이션 타이틀을 실제로 크게 보여줄 것인지 설정할 수 있습니다. 보통 여러 네비게이션 스택으로 이루어진 네비게이션 인터페이스에서 첫번째 혹은 두번째까지만 큰 타이틀을 보여줄 경우를 생각하면 됩니다.

\
searchController 에는 사용할 Search Controller를 셋팅해줍니다.

![](/assets/170626/201_whats_new_in_cocoa_touch6.jpg)

많은 기존의 개발자가 크기가 변하는 네비게이션바 영역에 대해서 걱정을 많이 할텐데요, 새로운 safeAreaInsets 을 사용하면 됩니다.

``` swift
class UIView {
  // auto layout
  var safeAreaLayoutGuide: UILayoutGuide { get }

  // manual layout
  var safeAreaInsets: UIEdgeInsets { get }
  func safeAreaInsetsDidChange()
}
```

Auto Layout 을 사용할 경우에는 safeAreaLayoutGuide 를 기준으로 뷰를 배치하면 되고,

Auto Layout 을 사용하지 않을 경우에는 safeAreaInsets 을 기준으로 작성합니다. safeArea 가 변경될 경우에는 safeAreaInsetsDidChange 가 불리게 됩니다.

``` swift
class UIScrollView {
  var contentInsetAdjustmentBehavior: UIScrollViewContentInsetAdjustmentBehavior
  var adjustedContentInset: UIEdgeInsets { get }
}
```

Scroll View 의 Content Inset 설정도 변화가 있습니다.

iOS 11 이전에는 Navigation Controller 안에 있는 UIScrollView 의 Content Inset 은 Navigation Controller 가 관여하여 셋팅했지만, iOS 11 에서는 Navigation Controller 가 더 이상 관여하지 않고 UIScrollView 가 알아서 셋팅하게 됩니다.


# Swift 4 and Foundation

![](/assets/170626/201_whats_new_in_cocoa_touch7.jpg)

전통적인 iOS 프로그래밍에서는 Data 를 Serialize 하는 방법으로 NSCoding 을 사용했습니다. 하지만 NSCoding 은 Swift Value 에는 사용할 수 없기 때문에, Swift 4 는 Codable 을 도입했습니다.

Codable 프로토콜을 추가하는 것만으로, 데이터의 Serialize 를 쉽게 할 수 있습니다. 특히나 Swift 구조체와 JSON 간의 변환을 쉽게 할 수 있기 때문에 매우 편리합니다.

---

Mac 과 iOS 프로그래밍에서 전통적으로 가장 중요했던 부분들 중 하나는 바로 KVC(Key-Value Coding)와 KVO(Key-Value Observing) 이었습니다.

Swift 4 에서는 이 기능들이 획기적으로 편리하게 Swift 스럽게 진화되었습니다.

![](/assets/170626/201_whats_new_in_cocoa_touch_kvc.jpg)

더 이상 Key Path 는 String 이 아닙니다.

![](/assets/170626/201_whats_new_in_cocoa_touch_kvc2.jpg)

![](/assets/170626/201_whats_new_in_cocoa_touch_kvc3.jpg)

위와 아래의 KVO 사용 방법을 비교해 보세요. Swift 4 에서는 KVO 가 정말 편리해졌습니다.

- What's New in Foundation 에서 더 자세한 내용을 알 수 있습니다.

---

![](/assets/170626/201_whats_new_in_cocoa_touch8.jpg)

아이폰에서는 디바이스의 상단 끝으로부터 아래로 스크롤했을 때 알림센터가 나오고, 최하단으로부터 위로 스크롤하면 제어 센터가 나오는 등, 앱이 아닌 iOS 시스템 자체의 제스쳐가 있어서 앱과 충돌을 일으킬 수 있습니다.

이 때 충돌을 방지하기 위해 기존에는 풀스크린 상태 등의 정보를 보고 OS 가 알아서 OS 의 제스쳐를 지연시키곤 했는데요, 이제 이 작동을 앱에서 직접 제어할 수 있습니다.


# Auto Layout and Scroll View

``` swift
class UIScrollView {
  var contentLayoutGuide: UILayoutGuide { get }
  var frameLayoutGuide: UILayoutGuide { get }
}
```

``` swift
imageView.centerXAnchor.constraint(equalTo: scrollView.contentLayoutGuide.centerXAnchor)
imageView.centerYAnchor.constraint(equalTo: scrollView.contentLayoutGuide.centerYAnchor)
```

iOS 의 가장 유명한 UI 중 하나는 제스쳐로 동작하는 이미지 뷰어입니다.

스크롤뷰 안에서 여러개의 이미지가 끝없이 이어지고, 두 개의 손가락으로 확대 축소하며, 가장 작게 축소했을 때는 이미지가 화면에 딱 맞게 축소되고 정확히 가운데에 정렬이 되죠. 하지만 이걸 제대로 구현하는 건 쉬운 일이 아니었습니다.

이제 Scroll View 에 Auto Layout 속성을 줄 수 있었습니다.

이제 Scroll View 안에서 가운데 정렬하기 등이 훨씬 쉬어졌습니다.


# Dynamic Type

![](/assets/170626/201_whats_new_in_cocoa_touch9.jpg)

![](/assets/170626/201_whats_new_in_cocoa_touch10.jpg)

Accessibility 는 매우 중요합니다. 그 중에서도 저시력자를 위한 폰트 크기를 대응하는 것은 매우 중요합니다.

iOS 11 에서는 더욱 더 큰 폰트를 표현할 수 있고, 그에 따른 UI 대응을 해야 합니다.

``` swift
let bodyFont = UIFont.preferredFont(forTextStyle: .body)
let titleFont = UIFont.preferredFont(forTextStyle: .title1)
```

텍스트 스타일에 따른 기본 폰트는 다음과 같이 가져옵니다.

그럼 커스텀 폰트는 어떻게 가져올까요?

``` swift
let bodyMetrics = UIFontMetrics(forTextStyle: .body)
let standardFont = ... // any font you want, for standard type size
let font = bodyMetrics.scaledFont(for: standardFont)
```

폰트의 크기가 직접 필요할 때는 다음과 같이 크기를 알 수 있습니다.

``` swift
let titleMetrics = UIFontMetrics(forTextStyle: .title3)
let standardHeight = ... // button height for standard type size
let height = titleMetrics.scaledValue(forValue: standardHeight)
```

![](/assets/170626/201_whats_new_in_cocoa_touch11.jpg)

위 아래로 배치된 베이스라인과의 정렬은 어떻게 해야 할까요?

iOS 11 에서는 System Spacing 을 기준으로 하는 많은 Auto Layout 함수들이 생겨났습니다.

SystemSpacing 이 포함된 Auto Layout 함수들을 확인해 보세요.

``` swift
NSLayoutConstraints.constraintWithVisualFormat(
  "V:|-[topLabel]-[bottomLabel]-|",
  options: [spacingBaselineToBaseline],
  metrics: nil,
  views: ...)

stackView.baselineRelativeArrangement = true
stackView.spacing = .spacingUseSystem
```

Visual Format 또는 Stack View 를 사용할 때는 위와 같이 Baseline 정렬을 사용할 수 있습니다.

아래 발표에 더 자세한 내용이 이어집니다.
-Building Apps with Dynamic Type


# Password AutoFill

![](/assets/170626/201_whats_new_in_cocoa_touch12.jpg)

지난 버전들에서 iOS 는 웹 상의 여러 로그인 정보를 저장하고 자동으로 채워주는 기능을 제공해 왔습니다.

하지만 네이티브 앱에서는 사용할 수가 없었기 때문에, 새로운 앱을 깔고 로그인하기가 어려웠던 기억이 다들 있을 겁니다.

이제 앱에서도 이 기능을 제공합니다. 키보드 위에 열쇠 버튼을 누르면 저장되어 있는 모든 사이트들의 리스트가 나오기 때문에, 적당한 로그인 정보를 선택할 수 있습니다.

물론 여러분의 앱에 맞는 로그인 정보를 바로 표시해 줄 수 있도록 할 수 있습니다.

다음 세션을 참고하세요.
- Introducing Password AutoFill for Apps


# Asset Catalogs

``` swift
class UIColor {
  init?(named name: String)
}
```

이제 Asset Catalog 에서 이미지 뿐만 아니라 컬러값을 관리할 수 있어서 named 함수로 Color 를 쉽게 사용할 수 있습니다.

![](/assets/170626/201_whats_new_in_cocoa_touch13.jpg)

Jpeg 를 1x, 2x, 3x 여러 개 사용하는 대신 PDF 벡터 이미지를 그대로 사용할 수 있게 됐습니다.

PDF 를 사용하면 아래와 같이 저시력자용 도움이 필요할 때에 즉시 사용할 수 있습니다.

![](/assets/170626/201_whats_new_in_cocoa_touch14.jpg)
