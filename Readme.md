# 클라이언트 힌트

## 클라이언트 테스트 준비

- 클라이언트 테스트를 하기 위해서 메타태그 삽입

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />

        <!-- 클라이언트 힌트 설정 -->
        <meta http-equiv="Accept-CH" content="device-memory, dpr, width, viewport-width, rtt, downlink, ect" />
        <meta http-equiv="Accept-CH-Lifetime" content="86400" />

        <title>클라이언트 힌트 테스트</title>
    </head>
    <body></body>
</html>

```

Accept-CH 헤더 속성은 클라이언트 측에서 요청할 사용자 정보를 정의합니다.

Accept-CH-Lifetime 헤더는 크롬 67에서 추가되어 Accept-CH 헤더에서 정의된 값을 전달하여 지정된 시간동안 캐싱해줍니다.

초 단위이므로 86400s = 1day 입니다.

- device-memory : 디바이스의 사용 가능한 메모리. 시스템의 RAM 용량과는 일치하지 않음
- dpr : device pixel ratio
- width : 요청된 리소스가 이미지일 경우, viewport에 표시되어 DPR이 적용된 css px 단위의 width
- viewport-width : css px 단위의 viewport width
- rtt : round-trip time
- downlink : 추정되는 다운로드 속도 (Mbit/s 단위)
- ect : 추정되는 connection type

실제로 화면을 띄워서 브라우저 header 속성을 보면 확인하실 수 있어요 😄

```html
<!-- 크롬 카나리아 84빌드 -->
<meta http-equiv="Accept-CH" content="UA, UA-Platform, UA-Arch, UA-Model, UA-Mobile, UA-Full-Version" />
```

다음은 크롬 카나리아 빌드에서 테스트 위한 코드입니다.

- UA : 웹브라우저의 이름과 버전 (Accept-CH 헤더와 관계없이 전송됨)
- UA-Platform : 플랫폼(OS)의 이름과 버전
- UA-Arch : 플랫폼(OS)의 아키텍쳐
- UA-Model : 기기 모델명
- UA-Mobile : 기기의 모바일 여부 “?0” 또는 “?1”
- UA-Full-Version : 웹브라우저의 버전 전체

### https 환경이 아니라 나오지 않을 수 있습니다.

### 이와 동시에 navigator.userAgentData 속성이 업데이트 될 예정입니다. (v8)

```javascript
// Low entropy values
navigator.userAgentData.mobile;
navigator.userAgentData.uaList;

// High entropy values
navigator.userAgentData.getHighEntropyValues([
  "platform",
  "platformVersion",
  "architecture",
  "model",
  "uaFullVersion"
]).then(res => console.log(res)); // Promise
```