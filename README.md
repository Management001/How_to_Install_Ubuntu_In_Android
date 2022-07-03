# How_to_Install_Ubuntu_In_Android
안드로이드에서 우분트를 설치하는 방법과 안드로이드 12에서 발생하는 signal 9 오류를 해결 하는 방법을 올리는 곳입니다.

# 안드로이드에 Linux 설치하기

## 개요

안드로이드 환경에서는 Termux라는 Linux 터미널 에뮬레이터를 이용해 여러 가지 기능들을 사용할 수 있다. Termux는 패키지 관리자를 지원하고 이를 통해 여러 기능의 구현이 가능하기 때문에 헤비 유저들이 애용하는 툴인데, 이러한 Termux를 이용해 할 수 있는 작업들 중에는 proo 패키지를 이용해 리눅스를 설치하는 것이 있다.

Proot은 chroot 기능을 사용할 수 있게 해주는 패키지인데 chroot을 이용하면 제한적이게나마 리눅스 환경을 Termux 내부에 구현할 수 있다. Chroot을 이용해 운영체제를 하위 경로에 띄울 수 있는지는 자세히 다루지 않지만, 아주 얕은 설명으로는 리눅스 권한을 잘 이용하고 정해진 경로에 필요한 파일들만 배치할 수 있다면 그걸 통해 하위에 운영체제가 돌아가는 것과 유사한 환경을 만들 수 있다고 보면 된다. 따라서 그래픽 가속만을 제외한다면 가상화는 아니기 때문에 성능 손실이 크지는 않다.

왜 굳이 안드로이드 태블릿에 리눅스를 올리려고 하는지에 대한 의문은 있을 수 있다. 물론 일반적인 사용 용도에서 이건 쓸데 없는 뻘짓이 맞고 이번 글에서 설명하는 리눅스 환경 역시 안되는 기능들이 꽤 많다 Docker, Snap 등과 같이 커널의 기능을 이용해야 되는 작업들은 사용할 수 없고 리눅스 환경에서조차 aarch64 프로그램의 수도 그렇게 많지 않기 때문에 리눅스 데스크탑을 쓰는 것만큼 많이 자유롭지는 않다.

그러나 코딩, 문서 작성과 같이 특정 사용 목적에 한해서는, 데스크톱 환경에서만 쓸 수 있는 응용 프로그램들을 사용할 수 있다면 같은 기기를 가지고도 더 높은 생산성을 얻을 수 있다는 장점이 있고, 하나의 하드웨어를 사용하더라도 해당 하드웨어를 활용하는 방법이 많으면 많을수록 유리한 것은 사용자이기 때문에 본 글의 의의가 완전히 없지는 않다고 판단한다.

본 글에서는 안드로이드 환경에서 리눅스를 구동하고 접근하기 위해 기반이 되는 앱인 Termux를 설치하고, Andronix를 통해 리눅스 환경을 구성한 뒤 VNC로 접속하는 방법에 대해 서술한 뒤 마지막으로 안드로이으 12 이상의 기기에서 백그라운드 프로세스 종료 없이 이를 활용하기 위한 Phantom Process Killer 비활성화 방법에 대해 알아보겠다.


## Termux 설치

![1656678457](https://user-images.githubusercontent.com/44454495/177044662-44dc27d7-e1b0-4e42-aecf-3d28c3834ffd.png)


먼저 Linux를 설치해보기 위해서는 안드로이드용 터미널 에뮬레이터인 Termux를 설치해야 한다. Termux는 구글 플레이 스토어 (이하 플스)에도 올라와있긴 하지만 플스의 버전은 오랫동안 업데이트되지 않은, 도태된 버전이다.

![1656678457](https://user-images.githubusercontent.com/44454495/177044667-fe9ef294-0938-4a17-ab02-269138988090.png)


Termux 의 최신 버전은 F-Droid를 통해 다운받을 수 있다. F-Droid는 일종의 서드파티 앱 스토어인데 자유 오픈소스 안드로이드 앱만을 다루는 스토어이다. 처음 들어 생소할 수도 있지만 믿을만한 곳이 걱정하지 말고 앱을 다운받으면 된다. F-Droid를 기기에 설치해 다운로드 받을 수도 있고, 웹 페이지에서 APK를 바로 다운로드 받아 설치할 수도 있다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044683-4ec9502d-43f9-4efb-9bbe-2f978b565e6f.png)


Termux를 처음 실행하면 이미지와 같은 커맨드라인 인터페이스 (이하 CLI)가 보일 것이다. 작업은 여기서 시작된다.

## Linux 배포판 (우분투 20.04) 설치하기

![1656678457](https://user-images.githubusercontent.com/44454495/177044692-a6b21aaa-f0ab-40fe-9093-4255f9c81fe3.png)

Termux에서 명령어만을 사용해 Linux를 설치할 수도 있지만 이건 꽤 귀찮은 작업이고, Androinix를 이용하면 자동화된 설치 스크립트로 여러 Linux 배포판들을 설치할 수 있다. 플스에서 Andronix를 받는다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044698-f1fa0944-ec42-4af1-8b37-f6ed16381413.png)

Linux는 오픈소스 OS로 Windows, Mac OS와는 달리 같은 기원을 가졌지만 서로 다른 기능과 생김새를 가진 배포판이라는 개념이 존재한다. 이 중 많이 사용되는 것이 Ubuntu라는 배포판인데, 좌측 위의 주황색 로고가 Ubuntu의 로고이다. 본 글에서는 Ubuntu의 설치에 대해 다뤄보고자 한다. Ubuntu 로고를 터치한다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044717-49e024a8-8171-4026-bc6f-64ec84d476f0.png)

이 창에서는 Androinix가 안드로이드에서 더 쓰기 편하게 수정한 Ubuntu 수정 배포판을 사용할 것인지, 아니면 일반 배포판을 사용할 것인지 고를 수 있는데 후자는 무료 오픈소스인데 반해 전자는 약 3,000원 정도에 유료로 제공되는 서비스이다. 본인의 경우 그렇게 큰 비용이 아니기도 하고 크게 손을 대기가 싫어서 결제를 하고 사용했다. 후자의 경우도 이후 과정이 크게 다르지는 않을 것으로 생각된다.

![1656678458](https://user-images.githubusercontent.com/44454495/177044727-24b8a75a-e654-4d1b-b776-a93c38f254c4.png)

유료든 무료든 하나를 정했다면 Ubuntu 버전을 정할 수 있다. Andronix의 배포판은 장기 지원 (LTS) 버전을 기준으로 제공되고 있는데, 가능하면 20.04를 추천한다. 18.04의 지원 기간이 그렇게 많이는 남지 않았고 이미 충분히 20.04로 넘어왔을 것이기 때문이다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044741-305cfd42-f7d3-4e3b-85a1-4dfde8933c01.png)

GUI 선택 창은 Ubuntu를 사용할 인터페이스를 선택하는 창이다. 데스크탑 UI가 필요없다면 CLI를 고르면 되지만, 일반적으로는 그냥 Desktop Environment를 선택하면 된다. 선택한 뒤 나오는 선택창에서는 XFCE를 선택할 것을 권장한다. QT를 사용해보지는 않았지만 왠지 안드로이드 환경에서는 조금 버거울 것 같은 느낌이 들기 떄문이다.

이후 링크가 하나 복사되는데 이걸 Termux 창에 붙여넣어 실행하면 된다. 실행 이후 설치 과정이 진행되는데 네트워크 환경과 기기 성능에 따라 30분 - 1시간 반 정도 시간이 걸린다. 다운로드 이후 계정명과 비밀번호를 설정해주면 GUI 기반으로 세부 설정을 진행하게 되는데 이에 대해서는 지역 설정을 Asia/Seoul로 해주는 것 외에는 다 기본값으로 설정해주면 될 것이다.

모든 설정이 끝난 뒤에는 Termux 터미널에 Unbuntu를 실행하기 위해서는 ./start-ubuntu.sh라는 명령어로 Shell 스크립트를 실행하면 된다. 이 스트립트의 이름은 유료 배포판을 이용했을 때는 ./start-andronix.sh이고, 배포판에 따라 명칭이 달라질 수 있다.

## VNC 서버 설정 및 접속

![1656856466](https://user-images.githubusercontent.com/44454495/177044759-7493e369-88e1-4683-abba-222134cfad84.png)

이제 VNC 서버를 설정해야 한다. 다른 기기는 모르곘지만, 갤럭시 탭 S8 울트라의 경우 VNC 접속시 화면에 선이 생기는 형상을 막기 위해 VNC 서버의 해상도 설정을 수정해줘야 했다.

nano /usr/local/bin/vncserver-start 명령어를 입력하면 이미지와 같은 화면이 나올 것이다. 여기서 수정된 부분이 case $CHOICE in... 부분인데 여기서 1번 선택지의 값을 GEO="-geometry 2960x1848" PORT=1로 수정하면 된다.

VNC 서버를 S8 울트라의 해상도로 실행하고 접속 포트틑 1로 설정한다는 의미로, Ctrl+x로 저장하고 나오면 된다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044769-db233ce7-81ff-4d5e-858c-bae08d2203b6.png)

vncserver-start 명령어를 입력하면 이미지와 같은 창이 뜨는데 S8 울트라 유저의 경우 방금 수정해준 1을 선택하면 된다. 다른 탭 사용자의 경우 다른 해상도를 선택해봐도 되나 이후 VNC 접속 후 화면에 검은 줄이 생긴다면 이전 과정을 각자의 기기 해상도에 맞게 따라하면 된다.

플스에서 VNC Viewer 앱을 다운받은 뒤 주소를 localhost:1로 설정하고 접속한다. 호스트 기기의 1번 포트에 접속한다는 의미이다.

![1656856466](https://user-images.githubusercontent.com/44454495/177044782-35c3f65f-de25-4ad7-9c88-a65562dd812c.png)

데스크톱 환경에 잘 접속한 것을 확인할 수 있다. VNC 서버 종료 명령어는 vncserver-stop으로 입력 후 이전에 VNC 서버를 실행한 포트 번호를 입력해주면 된다.

## Phantom Process Killer 임시 해제

여기까지면 다 좋은데, 안드로이드 12부터 까다로운 제한이 생겼다. Phantom Process Killer가 백그라운드에 상주하는 프로세스의 수를 제한한다는 점이다. 본래 이 기능은 안쓰는 프로세스를 종료함으로써 기기 최적화에 도움이 되는 기능이지만 이번 글과 같이 백그라운드에서 계속 프로세스를 돌려야 하는 Termux와 같은 경우에는 매우 까다로운 제한이다. 실제로 해제 없이 사용 시 Process Terminated Status:9와 같은 오류로 Termux가 종종 뻗는다.

이를 해제하기 위한 우회 방법은 영구적인 방법과 비영구적인 방법이 있는데 프로세스 제한을 끈다는 것이 끼칠 영향과, 영구 해제 시 반드시 업데이트 전 이를 재설정해줘야 한다는 점을 고려하면 비영구적인 방법을 택하는 것이 훨씬 합리적이다. 이 경우는 ADB권한 또는 루트 권한을 이용하는데, 후자의 경우 손해를 보는게 많기에 전자만을 언급한다.

ADB 드라이버를 깔고 기기의 개발자 설정에서 USB 디버깅을 활성화한 뒤 다음 명령어를 입력해준다:

```
adb shell "/system/bin/device_config put activity_manager max_phantom_processes 2147483647"
```

이 때 유의할 점은 이렇게 해제한 상태는 기기가 켜져있는 경우에만 유지된다는 것이다. 즉, 기기를 재부팅할 경우에는 다시 PC에서 ADB 명령어를 입력해줘야 한다. 또한 작동 방식상 기기를 부팅하고 잠금을 해제한 뒤 약 5 - 10분쯤 뒤에 이 명령어를 입력해야 적용이 된다고 하는데 이 역시 유의해야 한다.

안드로이드 12L과 13에서는 Phantom Process Killer를 끄는 옵션이 개발자 옵션에 들어가 편하게 해제가 가능하다고 하는데, 업데이트가 이뤄지기 전까지는 위와 같은 임시처방 말고는 답이 없을 것으로 보인다.

---
# 안드로이드 adb 설치 및 설정 방법

Android ADB (Android Debug Bridge)는 PC와 스마트 폰 간에 통신을 할 수 있는 명령어도 도구입니다. 안드로이드 개발자에게는 apk 설치, log 출력의  등의 개발에 많은 활동에서 adb를 거의 매일 사용하고 있습니다. 또한 디버깅 목적뿐 아니라 스마트 폰 화면을 PC로 미러링 할 수 있는 App에서도 adb를 사용합니다. 예를 들어 scrcpy 와 Mirroid 등 같은 스마트 폰 미러링 앱을 사용하기 위해서는 PC에 adb를 미리 설치해야 합니다.  (※ Mrriod는 adb.exe가 설치 파일에 포함됨)

본 포스팅은 상용 단말 (=User version) 기준으로 ADB 연결을 설정하는 과정을 설명하고 요약하면 다음과 같습니다.

1. 스마프 폰 제조사 USB 드라이버를 설치 (각 제조사 홈페이지에서 USB 통합 Driver를 제공함, 링크는 다음과 같습니다. 삼성 통합드라이버, LG USB Driver , Google USB Driver) (필수)
2. PC에서 ADB Tool 다운로드:  최신 버전 Android SdK platform tool 설치 (다운로드 링크)
3. PC 환경 변수에 ADB 설치 Path 추가
4. 스마트 폰의 개발자 메뉴에서 'USB 디버깅' 연결 활성화
5. 스마트 폰과 PC 연결 시 USB 디버깅 허용 

삼성 통합드라이버 다운로드 주소
https://downloadcenter.samsung.com/content/SW/202012/20201229125900126/SAMSUNG_USB_Driver_for_Mobile_Phones.exe

LG USB Driver 다운로드
https://www.lge.co.kr/lgekor/download-center/downloadCenterList.do#cur

Goole USB Driver 다운로드
https://developer.android.com/studio/run/win-usb?hl=ko

※ Windows용과 MAC용 LG USB 통합 드라이버(LG United Mobile Driver)를 첨부 파일로 업로드 하였습니다.

## 1. PC에서 ADB Tool 다운로드: 다운로드 링크
Android Studio SDK를 설치하는 경우에도 SDK manager를 통해서 다운로드 및 설치할 수 있고, 독립형 패키지로도 Andorid SDK platform-tools을 다운로드하여 adb를 설치할 수 있습니다.  이는 링크에서 다운로드가 가능하며 다운로드한 zip 파일(platform-tools_r30.0.5-windows.zip)을 압축 해제하면 됩니다.
출처: https://kibua20.tistory.com/165 [모바일 SW 개발자가 운영하는 블로그:티스토리]

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045190-eb6a770b-e656-4583-82ba-948ac6eb907b.png)

## 2. PC(Windows OS)에서 ADB Path를 환경 변수에 추가

최신 버전의 Android SDK Tool을 다운로드하여 설치를 원하는 폴더에 압축을 풉니다. 임의의 폴더에서도 adb를 실행할 수 있도록 환경 변수 PATH에 adb 설치 경로를 추가합니다.
 
1. Android SDK Tool을 통해서 platform-tools_r30.0.5-windows.zip 파일을 원하는 폴더에 압축해제
2. 내 컴퓨터의 고급 시스템 설정 (제어판-시스템 보안-시스템-고급 시스템 설정 또는 내 컴퓨터에서 마우스 오른쪽 키)
3. 시스템 속성 메뉴 -고급 탭에서 '환경 변수' 선택
4. 시스템 변수 (또는 사용자 변수)에서 Path 편집
5. Path 항목에 platform-tools을 설치한 경로를 추가 

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045233-7ae11989-4755-41eb-9679-762064c4ea3b.png)

## 3. 우분투/Mac OS에서 ADB 설치
Android SDK를 설치하면 adb가 포함된 Plaftform Tools까지 같이 설치지만, SDK 설치 없이 ADB만 별도로 패키지 매니져로  설치가 가능합니다.  우분투와 Mac에서는 별도의 Path설정은 필요 없습니다.

우분투OS에서 ADB 설치 명령어 
```
$ sudo apt install adb 
```


Mac OS에서 adb설치 명령어
```
$ brew install android-platform-tools
```

## 4. 스마트폰에서 USB 디버깅 연결 활성화 

User Debug 버전에서 USB 디버깅 옵션 메뉴가 기본 값으로 활성화 상태이지만, 일반 상용 버전 (User 버전)은 개발자 메뉴에서 디버깅 옵션을 활성화시켜야 합니다. 

- 개발자 메뉴 활성화: 시스템 세팅 - 휴대폰 정보 - 소프트웨어 정보- 빌드 번호를 3번 누르기 
- 개발자 메뉴에서 "USB 디버깅 옵션" 활성화 

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045318-c53a681f-6752-488c-b94b-55d59071d838.png)

스마트폰에서 USB 디버깅 연결 활성화 

## 5. USB 연결 후 adb 정상 동작 확인 

Win+R키를 누르고 명령 프롬프트 창에서 $ adb devices 입력합니다.  스마트 폰 화면에서는 USB 허용 팝업을 표시하고 허용해야 연결됩니다. 

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045342-6731c1e9-1f75-42a6-ad4d-63ed107bda5c.png)

스마트 폰에서 USB 연결 허용

$ adb devices 실행 시 연결된 Device list를 표시하면 정상적으로 동작하는 것입니다.  adb는 상당히 많은 기능을 가지고 있습니다. 각 세부 기능은 Google 공식 문서를 참고하세요.


Google 공식 문서

https://developer.android.com/studio/command-line/adb?hl=ko

PC에서  adb 연결 확인

## 6. Adb 에러에 대한 조치 사항

__[에러 1] USB 연결 상태에서 adb devices 결과에 스마트 폰 검색이 되지 않는 경우__
adb가 정상적으로 설치한 상태에서 $ adb devices에서 단말 List가 보이지 않는 경우 핸드폰 제조사 USB 드라이드가 설치되지 않아서 발생하는 것입니다. 각 제조사에 맞는 USB 통합 드라이버를 설치하면 해결이 됩니다.

- 삼성 통합드라이버 다운로드
- LG USB Driver 다운로드
- Goole USB Driver 다운로드

 
__[에러 2] USB 연결 상태에서 adb devices 결과가 'unauthorized' 에러가 발생하는 경우__
adb가 정상적으로 설치한 상태에서 $ adb devices에서 unauthorized 가 표시되는 경우 스마트 폰에서 "USB 디버깅 허용"을 선택합니다.

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045534-a5723fcf-a241-41ea-b9c7-e3b5b8e0291a.png)

USB 연결 상태에서 adb devices 결과가 'unauthorized' 에러가 발생하는 경우
 
__[에러 3] Mirroid 실행 상태에서 ADB 실행 에러 조치 방법__
PC에서 2개 이상의 adb가 설치된 경우입니다.  예를 들어 Mirroid 프로그램에서도 adb가 설치하고, Andorid SDK에서도 다른 버전의 Adb가 설치가 되었다면  Adb 실행 시 Version Mismatch 에러 ("adb server version (40) doesn't match this client (41); killing...​") 가 발생할 수 있습니다.
 
이 경우 여러 경로에 설치된 가장 최 상위의 adb 버전으로 통일하면 문제가 해결됩니다.  미러로이드와 같은 경우 ADB 파일(adb.exe, AdbWinApi.dll, AdbWinUsbApi.dll)파일을 미러로이드 설치폴더(C:\Program Files (X86)\Mirroid)로 덮어 쓰면 문제가 해결됩니다.
 
에러 메시지 
```
C:\User> adb shell  
adb server version (40) doesn't match this client (41); killing...​
* daemon started successfully 
```

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045574-cf41f13e-3f6b-45d0-be03-0593fd6d41a5.png)

Mirroid 실행 상태에서 ADB 실행 에러

조치 방법

최신 버전의 adb 파일 (adb.exe, AdbWinApi.dll, AdbWinUsbApi.dll)으로 통일한다.

![img1 daumcdn](https://user-images.githubusercontent.com/44454495/177045586-fd0752be-57cc-49f0-83dd-76893917e93c.png)

adb server version (40) doesn't match this client (41); killing..에러 조치 방법
 
__[에러 4] ADB 실행 시 daemon not running 에러 조치 방법__
adb devices 연결 시도 시  adb daemon이 정상적으로 실행해야 하지만, daemon not running 에러 메시지가 출력된다면 이전에 실행한  adb daemon이 정상적으로 종료되지 않아 Process가 살아 있기 때문에 발생하는 에러입니다.  이 경우 adb kill-server로  daemon을 kill 하던가, TaskManager에서 해당 process를 모두 kill 해야 합니다.  위와 같이 조치해도 동일한 문제가 발생하면 logout후 또는 재부팅해야 합니다. 

에러 메시지 
``` 
* daemon not running; starting now at tcp:5037
* daemon started successfully 
```
 
조치 방법 
```
$ adb kill-server
$ adb devices
--> daemon start message

```

출처: https://kibua20.tistory.com/165 [모바일 SW 개발자가 운영하는 블로그:티스토리]

---

# 우분트 한글패치 설치하기

## 사전 작업

- 리눅스 unzip 설치하기
https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_unzip_%EC%84%A4%EC%B9%98

- 설치

__Bash__

```
wget http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip
unzip NanumFont_TTF_ALL.zip -d NanumFont
rm -f NanumFont_TTF_ALL.zip
mv NanumFont /usr/share/fonts/
fc-cache -f -v
```

실행 예시

__Console__

```
root@localhost:~# wget http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip
--2021-09-22 06:11:51--  http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip
Resolving cdn.naver.com (cdn.naver.com)... 211.216.46.16, 125.209.207.10, 125.209.207.38, ...
Connecting to cdn.naver.com (cdn.naver.com)|211.216.46.16|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14946146 (14M) [application/zip]
Saving to: ‘NanumFont_TTF_ALL.zip’

NanumFont_TTF_ALL.zip     100%[===================================>]  14.25M  5.92MB/s    in 2.4s

2021-09-22 06:11:54 (5.92 MB/s) - ‘NanumFont_TTF_ALL.zip’ saved [14946146/14946146]
```

__Console__

```
root@localhost:~# unzip NanumFont_TTF_ALL.zip -d NanumFont
Archive:  NanumFont_TTF_ALL.zip
  inflating: NanumFont/NanumBrush.ttf
  inflating: NanumFont/NanumGothic.ttf
  inflating: NanumFont/NanumGothicBold.ttf
  inflating: NanumFont/NanumGothicExtraBold.ttf
  inflating: NanumFont/NanumMyeongjo.ttf
  inflating: NanumFont/NanumMyeongjoBold.ttf
  inflating: NanumFont/NanumMyeongjoExtraBold.ttf
  inflating: NanumFont/NanumPen.ttf
```

__Console__

```
root@localhost:~# rm -f NanumFont_TTF_ALL.zip
root@localhost:~# mv NanumFont /usr/share/fonts/
root@localhost:~# fc-cache -f -v
/usr/share/fonts: caching, new cache contents: 0 fonts, 5 dirs
/usr/share/fonts/NanumFont: caching, new cache contents: 8 fonts, 0 dirs
...
/root/.local/share/fonts: skipping, no such directory
/root/.fonts: skipping, no such directory
/var/cache/fontconfig: cleaning cache directory
/root/.cache/fontconfig: not cleaning non-existent cache directory
/root/.fontconfig: not cleaning non-existent cache directory
fc-cache: succeeded
```

---

## 마치며

이후로 Linux 환경을 사용하는 것은 사용자에게 달렸다. 앞서 언급한 것과 같이 이렇게 구성된 Linux 환경은 여전히 한계가 많다. Docker, Snap과 같은 유용한 툴들을 사용하지 못하고, 프로그램에 따라서도 업데이트 이후 작동이 안되는 경우가 생길 수 있다. (황당하게도 apt-update로 브라우저 업데이트시 브라우저가 뻗는다. 복구 방법은 Andronix Browser 문서에 적혀 있다.)

그럼에도 불구하고 태블릿과 같은 대화면 기기에서 데스크톱 환경에서만 사용할 수 있는 응용 프로그램을 사용할 수 있다는 것은 생산성 개선과 활용 측면에서 사용자에게 큰 자율성을 부여할 수 있기 때문에 그 점에서 Termux를 이용한 Linux 구동은 의의를 가진다고 판단한다. 사실 이렇게 사용자가 제조사가 원하는 의도에서 벗어나 다양한 방법으로 사용할 수 있다는 측면에서 개인적으로는 안드로이드 OS를 애플의 iOS/iPadOS보다 선호한다. 이견이 있을 수는 있겠지만, 적어도 Geek인 본인에게는 그러한 기기들이 '재미가 없다'.

다른 사용자들에게 많은 도움이 된다면 좋겠다.

## 참고자료

1. [안드로이드 태블릿에서 코딩하기 프로그래밍하기 - 안드로이드에서 우분투 리눅스로 코딩하기](https://m.blog.naver.com/einsbon/222302444210)
2. [Andronix Document](https://docs.andronix.app/)
3. [Android Phantom, Cached And Empty Processes](https://gist.github.com/agnostic-apollo/dc7e47991c512755ff26bd2d31e72ca8)

출처: https://gall.dcinside.com/board/view/?id=tabletpc&no=898108 [안드로이드에 Linux 설치하기 - 디시인사이드 갤러리]
