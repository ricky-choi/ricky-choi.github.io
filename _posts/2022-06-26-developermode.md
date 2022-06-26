---
layout: post
title:  "Developer Mode"
date:   2022-06-26 15:13:00 +0900
categories: wwdc wwdc22
---

iOS 16 에서 Developer Mode 가 생겼습니다.

개발 환경에서 발생할 수 있는 보안 위협에 대응하기 위해서라고 하네요.

대부분의 일반 사용자는 물론 개발 환경이 필요하지 않습니다.

말 그대로 Run, Debug, Instrument 등 local 개발 환경에서만 필요하고, Testflight, Enterprise 배포 등에서는 필요하지 않습니다.

# 켜는 법

## devmodectl

macOS ventura 에서 devmodectl 을 사용할 수 있습니다.

연결된 디바이스가 있을 때,

연결된 디바이스의 Developer Mode 상태 보기
``` bash
$ devmodectl list
```
연결된 모든 디바이스에 대해 Developer Mode 켜기
``` bash
$ devmodectl streaming
```

하나의 디바이스만 Developer Mode 켜기
``` bash
$ devmodectl single 'UDID'
```

하지만 이때 디바이스에 passcode 가 설정되어 있지 않아야 하기 때문에 유용한지는 잘 모르겠네요. 물론 아무 디바이스나 Developer Mode 가 켜지는 걸 방지하기 위한걸로 보입니다.

## 디바이스에서 Developer Mode 키거나 끄기

> 설정 > 개인정보 보호 및 보안 > 개발자 모드

에서 키거나 끕니다.