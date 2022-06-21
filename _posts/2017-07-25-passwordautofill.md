---
layout: post
title:  "[WWDC17] 로그인 정보 불러오기"
date:   2017-07-25 12:21:00 +0900
categories: wwdc wwdc17
---

앱이나 웹을 사용할 때 가장 큰 허들 중에 하나는 서비스에 로그인하는 일입니다.

사파리에서는 오래 전부터 자동 로그인 기능을 지원해 왔지만, 앱에서는 OS에서 제공하는 자동 로그인 기능이 없었기 때문에, 꽤 불편했었습니다. 

아마도 여러분은 앱에서 로그인하려다가 암호가 생각이 나지 않아서 사파리 혹은 크롬의 암호 목록을 뒤져본 일이 있으실 겁니다.

물론 이렇게 할 수도 있지만, 너무나 불편했습니다.

로그인의 허들은 많은 유저가 앱을 사용해보지도 못하고 포기하게 만들 수도 있습니다.

\
이제 앱에서도 사파리에 저장되어 있는 로그인 정보에 보다 쉽게 접근할 수 있게 되어 사파리에서 자동 로그인하던 똑같은 경험을 사용자에게 제공할 수 있습니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_0.jpg)

> 사파리에서는 최초 로그인할 때 로그인 정보를 저장할 수 있습니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_1.jpg)

> 다음 번에 웹페이지를 방문하면 로그인 정보를 자동으로 입력해 줍니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_2.jpg)

iOS 11에서는 데스크탑의 사파리와 마찬가지로 설정에서 모든 로그인 정보를 확인할 수 있으며, 반드시 Touch ID또는 디바이스의 암호가 필요합니다.

암호는 iCloud Keychain 에 암호화되어 저장되며, 애플은 절대로 이 정보를 볼 수 없습니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_5.jpg)

iOS 11에서 앱의 로그인 화면입니다.

키보드 바로 위에 저장되어 있는 로그인 정보가 바로 뜨는 걸 볼 수 있습니다.

이 부분을 QuickType bar 라고 하며, 자동으로 보여진 로그인 정보를 터치해서 로그인 정보를 입력하고 바로 서비스에 로그인 할 수 있습니다.

이 작업은 대부분의 앱에서 자동으로 작동합니다.

개발자들은 사실 연계된 웹사이트가 있다면 다른 어떤 작업도 하지 않아도 됩니다.

> Password Autofill은 대부분의 경우 자동으로 작동합니다

![](/assets/170725/image_8125612811500862595148.png)

> 아직 iOS 11 정식 출시 전이라 특별한 처리를 하지 않은 트위터 앱이지만, 로그인 정보를 바로 입력할 수 있습니다.

하지만 분명히 더 정확히 작동하게 하기 위한 단계가 존재합니다.

이제 Password Autofill 을 올바로 작동하기 위한 다음 네가지방법을 알아보겠습니다.

1. 퀵타입 바 보여주기
2. 로그인 정보를 적절한 필드에 채워주기
3. 퀵타입 바에 올바른 로그인정보 보여주기
4. 3rd 파티 앱으로 로그인하기

# 퀵타입 바 보여주기

퀵타입 바를 보여주기 위해선 다음 조건을 만족시켜야 합니다.
- 저장되어 있는 로그인 정보가 있어야 합니다.
- 로그인 정보가 입력될 UITextField 또는 UITextView가 있어야 합니다. UITextField 또는 UITextView가 아닌 커스텀 인풋 컨트롤을 쓰고 있다면 UITextInput protocol 을 준수하도록 만들면 됩니다. 물론 UITextField와 UITextView는 UITextInput을 준수하는 컨트롤입니다.
- 저장되어 있는 정보가 있고, iOS가 UITextInput 컨트롤러가 로그인과 관련되어 있다고 판단하면 자동으로 퀵타입바를 보여줍니다.
- iOS가 자동으로 판단하게 하지 않고, 좀 더 확실하게 퀵타입 바를 보여주려면, textContentType을 사용하면 됩니다.

# 로그인 정보를 적절한 필드에 채워주기

![](/assets/170725/2017206_introducing_password_autofill_for_apps_8.jpg)

textContentType의 종류는 위와 같습니다.

textContentType은 텍스트 필드에 의미를 보여해서 해당하는 종류의 데이터를 자동으로 입력하는데 도움을 줍니다.

textContentType은 iOS 10에서 도입되었으며 WWDC16의 세션 'Increase Usage of Your App with Proactive Suggestions' 에서 소개하고 있습니다.

\
iOS 11에서는 textContentType에 .username과 .password가 추가되었으며, 퀵타입 바에 로그인 정보를 자동으로 보여주는 역할을 하게 됩니다.

textContentType은 코드에서 직접 입력하거나 인터페이스 빌더를 통해 설정할 수 있습니다.

textContentType을 사용하면 로그인이름과 암호를 반드시 한 화면 안에 구현하지 않아도 됩니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_10.jpg)

![](/assets/170725/2017206_introducing_password_autofill_for_apps_11.jpg)

![](/assets/170725/2017206_introducing_password_autofill_for_apps_12.jpg)

![](/assets/170725/2017206_introducing_password_autofill_for_apps_13.jpg)

textContentType에 .username 또는 .password를 설정해주면, 위와 같이 열쇠 모양의 버튼이 있는 퀵타입 바가 나타나게 되며, 이 버튼을 이용해 저장된 로그인 정보들 중 적절한 항목을 선택해서 자동으로 정보를 입력할 수 있습니다.

(참고로 Touch ID를 확인하는 단계에서는 앱이 inactive 됩니다.)

\
하지만 로그인 할 정보를 찾는 일은 많이 번거럽죠.

우리 앱에 맞는 로그인 정보를 바로 보여줄 수 있는 방법을 알아보겠습니다.

# 퀵타입 바에 올바른 로그인정보 보여주기

![](/assets/170725/2017206_introducing_password_autofill_for_apps_16.jpg)

퀵타입 바에 올마른 정보를 표시해주기 위해선 해당 앱이 특정 웹사이트와 연계되어 있다는 사실을 앱이 확인해야 합니다.

이 과정은 iOS 8에서 소개되었던 Shared Web Credentials 설정법과 동일합니다.

이 때, 앱과 웹 연계 설정을 해두었으면 지금 설명하고 있는 AutoFill 기능도 iOS의 heuristic 기능과 맞물려 자동으로 적용되게 됩니다. 이런 이유로 앞에 보여드렸던 트위터 앱도 AutoFill 이 자동으로 적용되고 있었습니다.

![](/assets/170725/safari_credentials.jpg)

사실 AutoFill 기능은 iOS 8부터 사용이 가능했습니다. 사파리로부터 로그인 정보를 가져오거나, 업데이트도 할 수 있습니다. WWDC14의 자료를 꼭 확인해보세요. iOS 11에서는 이 과정이 더욱 쉬워지고, 보안이 더 강화된 모습을 볼 수 있습니다.

---

이제 앱과 웹을 연계시키는 방법을 알려드리겠습니다.

Xcode Target > Capabilities > Associated Domains 를 키고 webcredentials:<your-domain> 을 입력합니다.

이 작업을 하면 entitlements 파일이 자동으로 생성 또는 갱신되고, app ID에 associated domains 항목이 추가됩니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_21.jpg)

app ID 설정이 실패하게될 경우 Apple Developer 사이트에서 직접 해당 항목을 추가해주면 됩니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_22.jpg)

![](/assets/170725/2017206_introducing_password_autofill_for_apps_23.jpg)

다음과 같은 내용이 적힌 텍스트 파일을 만듭니다. 파일 이름은 apple-app-site-association 입니다.

``` json
{
  "webcredentials": {
    "apps": [ "E5336VM85F.com.example.Shiny" ]
  }
}
```

E5336VM85F 는 Team ID 입니다. 뒤 이어 앱의 bundleID 를 적습니다.

\
이 파일은 해당 서버의 정해진 위치에 놓습니다. 다음 두 가지 위치 모두 가능하며 첫 번째 위치를 조금 더 추천합니다.(https만 사용이 가능합니다.)

```
https://example.com/.well-known/apple-app-site-association
https://example.com/apple-app-site-association
```

보다 자세한 내용은 다음 링크를 참고하셔도 됩니다.

[Shared Web Credentials][shared-web-credentials]

---

설정을 다 한 후에는 앱을 디버깅하면서 적절히 셋팅이 되었는지 확인이 가능합니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_26.jpg)

![](/assets/170725/swcd_console.png)

> 콘솔 앱에서 swcd 로 검색하면 앱을 설치, 삭제, 업데이트 할 때 그리고 디버깅 중 정보를 볼 수 있습니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_27.jpg)

> 저장된 로그인 정보들은 설정에 새로운 항목으로 추가되었습니다.

# 3rd 파티 앱으로 로그인하기

요즘은 페이스북이나 트위터, 구글, 카카오, 네이버 등 많은 메이저 회사에서 로그인 솔루션을 제공합니다.

이런 서비스들을 통해 로그인하는 방법은 Safari View Controller 를 사용하는게 가장 좋습니다.

이 방법들에 대해선 다른 세션에서 더 상세한 내용이 이어집니다.

![](/assets/170725/2017206_introducing_password_autofill_for_apps_29.jpg)




[shared-web-credentials]: https://developer.apple.com/documentation/security/shared_web_credentials