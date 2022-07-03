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

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d512ad99367537971eaa8d14aa68b3fc90188fb41d7c2b806414241737d8933e5b26a701


먼저 Linux를 설치해보기 위해서는 안드로이드용 터미널 에뮬레이터인 Termux를 설치해야 한다. Termux는 구글 플레이 스토어 (이하 플스)에도 올라와있긴 하지만 플스의 버전은 오랫동안 업데이트되지 않은, 도태된 버전이다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d5229dd9366115264e9086b73c868d21055e33e013a7242ac3cf74d2

Termux 의 최신 버전은 F-Droid를 통해 다운받을 수 있다. F-Droid는 일종의 서드파티 앱 스토어인데 자유 오픈소스 안드로이드 앱만을 다루는 스토어이다. 처음 들어 생소할 수도 있지만 믿을만한 곳이 걱정하지 말고 앱을 다운받으면 된다. F-Droid를 기기에 설치해 다운로드 받을 수도 있고, 웹 페이지에서 APK를 바로 다운로드 받아 설치할 수도 있다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d562cdc937459647bf3f9a70d32aebb80a1acec239a46f0a690903

Termux를 처음 실행하면 이미지와 같은 커맨드라인 인터페이스 (이하 CLI)가 보일 것이다. 작업은 여기서 시작된다.

## Linux 배포판 (우분투 20.04) 설치하기

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d522ada9367537971ea12eddb5e5a710f66ca6bad7a4cbcacdd61dfded04bb276b82b784

Termux에서 명령어만을 사용해 Linux를 설치할 수도 있지만 이건 꽤 귀찮은 작업이고, Androinix를 이용하면 자동화된 설치 스크립트로 여러 Linux 배포판들을 설치할 수 있다. 플스에서 Andronix를 받는다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d522dd99361527264e95dfb7f97b9174e725d125bfd707478e535913b5

Linux는 오픈소스 OS로 Windows, Mac OS와는 달리 같은 기원을 가졌지만 서로 다른 기능과 생김새를 가진 배포판이라는 개념이 존재한다. 이 중 많이 사용되는 것이 Ubuntu라는 배포판인데, 좌측 위의 주황색 로고가 Ubuntu의 로고이다. 본 글에서는 Ubuntu의 설치에 대해 다뤄보고자 한다. Ubuntu 로고를 터치한다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d522cdc9361527264e93e809691274fb5920533d9baf5b413d17dc79e6

이 창에서는 Androinix가 안드로이드에서 더 쓰기 편하게 수정한 Ubuntu 수정 배포판을 사용할 것인지, 아니면 일반 배포판을 사용할 것인지 고를 수 있는데 후자는 무료 오픈소스인데 반해 전자는 약 3,000원 정도에 유료로 제공되는 서비스이다. 본인의 경우 그렇게 큰 비용이 아니기도 하고 크게 손을 대기가 싫어서 결제를 하고 사용했다. 후자의 경우도 이후 과정이 크게 다르지는 않을 것으로 생각된다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d5329d89361527264e91d436c9ddcdb8f3bb057c836bc7b3e3516aaa27

유료든 무료든 하나를 정했다면 Ubuntu 버전을 정할 수 있다. Andronix의 배포판은 장기 지원 (LTS) 버전을 기준으로 제공되고 있는데, 가능하면 20.04를 추천한다. 18.04의 지원 기간이 그렇게 많이는 남지 않았고 이미 충분히 20.04로 넘어왔을 것이기 때문이다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70b0d5329dd9361527264e91c9423ad0f37c64d2f8da9329eca8b15a1f0bb8

GUI 선택 창은 Ubuntu를 사용할 인터페이스를 선택하는 창이다. 데스크탑 UI가 필요없다면 CLI를 고르면 되지만, 일반적으로는 그냥 Desktop Environment를 선택하면 된다. 선택한 뒤 나오는 선택창에서는 XFCE를 선택할 것을 권장한다. QT를 사용해보지는 않았지만 왠지 안드로이드 환경에서는 조금 버거울 것 같은 느낌이 들기 떄문이다.

이후 링크가 하나 복사되는데 이걸 Termux 창에 붙여넣어 실행하면 된다. 실행 이후 설치 과정이 진행되는데 네트워크 환경과 기기 성능에 따라 30분 - 1시간 반 정도 시간이 걸린다. 다운로드 이후 계정명과 비밀번호를 설정해주면 GUI 기반으로 세부 설정을 진행하게 되는데 이에 대해서는 지역 설정을 Asia/Seoul로 해주는 것 외에는 다 기본값으로 설정해주면 될 것이다.

모든 설정이 끝난 뒤에는 Termux 터미널에 Unbuntu를 실행하기 위해서는 ./start-ubuntu.sh라는 명령어로 Shell 스크립트를 실행하면 된다. 이 스트립트의 이름은 유료 배포판을 이용했을 때는 ./start-andronix.sh이고, 배포판에 따라 명칭이 달라질 수 있다.

## VNC 서버 설정 및 접속

1ebec223e0dc2bae61abe9e74683766d1b166dbef70a0e5928dd937459647bf392e204881932a88a3d50009e903ef949969

이제 VNC 서버를 설정해야 한다. 다른 기기는 모르곘지만, 갤럭시 탭 S8 울트라의 경우 VNC 접속시 화면에 선이 생기는 형상을 막기 위해 VNC 서버의 해상도 설정을 수정해줘야 했다.

nano /usr/local/bin/vncserver-start 명령어를 입력하면 이미지와 같은 화면이 나올 것이다. 여기서 수정된 부분이 case $CHOICE in... 부분인데 여기서 1번 선택지의 값을 GEO="-geometry 2960x1848" PORT=1로 수정하면 된다.

VNC 서버를 S8 울트라의 해상도로 실행하고 접속 포트틑 1로 설정한다는 의미로, Ctrl+x로 저장하고 나오면 된다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70a09512cd2937459647bf37bf3b1e4e252230e851b66ab97391022ff10

vncserver-start 명령어를 입력하면 이미지와 같은 창이 뜨는데 S8 울트라 유저의 경우 방금 수정해준 1을 선택하면 된다. 다른 탭 사용자의 경우 다른 해상도를 선택해봐도 되나 이후 VNC 접속 후 화면에 검은 줄이 생긴다면 이전 과정을 각자의 기기 해상도에 맞게 따라하면 된다.

플스에서 VNC Viewer 앱을 다운받은 뒤 주소를 localhost:1로 설정하고 접속한다. 호스트 기기의 1번 포트에 접속한다는 의미이다.

1ebec223e0dc2bae61abe9e74683766d1b166dbef70a095629d99376725540ef1d18db819aecd7bdb549beffdf4af4dd14f1229c8411

데스크톱 환경에 잘 접속한 것을 확인할 수 있다. VNC 서버 종료 명령어는 vncserver-stop으로 입력 후 이전에 VNC 서버를 실행한 포트 번호를 입력해주면 된다.

## Phantom Process Killer 임시 해제

여기까지면 다 좋은데, 안드로이드 12부터 까다로운 제한이 생겼다. Phantom Process Killer가 백그라운드에 상주하는 프로세스의 수를 제한한다는 점이다. 본래 이 기능은 안쓰는 프로세스를 종료함으로써 기기 최적화에 도움이 되는 기능이지만 이번 글과 같이 백그라운드에서 계속 프로세스를 돌려야 하는 Termux와 같은 경우에는 매우 까다로운 제한이다. 실제로 해제 없이 사용 시 Process Terminated Status:9와 같은 오류로 Termux가 종종 뻗는다.

이를 해제하기 위한 우회 방법은 영구적인 방법과 비영구적인 방법이 있는데 프로세스 제한을 끈다는 것이 끼칠 영향과, 영구 해제 시 반드시 업데이트 전 이를 재설정해줘야 한다는 점을 고려하면 비영구적인 방법을 택하는 것이 훨씬 합리적이다. 이 경우는 ADB권한 또는 루트 권한을 이용하는데, 후자의 경우 손해를 보는게 많기에 전자만을 언급한다.

ADB 드라이버를 깔고 기기의 개발자 설정에서 USB 디버깅을 활성화한 뒤 다음 명령어를 입력해준다:

adb shell "/system/bin/device_config put activity_manager max_phantom_processes 2147483647"

이 때 유의할 점은 이렇게 해제한 상태는 기기가 켜져있는 경우에만 유지된다는 것이다. 즉, 기기를 재부팅할 경우에는 다시 PC에서 ADB 명령어를 입력해줘야 한다. 또한 작동 방식상 기기를 부팅하고 잠금을 해제한 뒤 약 5 - 10분쯤 뒤에 이 명령어를 입력해야 적용이 된다고 하는데 이 역시 유의해야 한다.

안드로이드 12L과 13에서는 Phantom Process Killer를 끄는 옵션이 개발자 옵션에 들어가 편하게 해제가 가능하다고 하는데, 업데이트가 이뤄지기 전까지는 위와 같은 임시처방 말고는 답이 없을 것으로 보인다.

## 마치며

이후로 Linux 환경을 사용하는 것은 사용자에게 달렸다. 앞서 언급한 것과 같이 이렇게 구성된 Linux 환경은 여전히 한계가 많다. Docker, Snap과 같은 유용한 툴들을 사용하지 못하고, 프로그램에 따라서도 업데이트 이후 작동이 안되는 경우가 생길 수 있다. (황당하게도 apt-update로 브라우저 업데이트시 브라우저가 뻗는다. 복구 방법은 Andronix Browser 문서에 적혀 있다.)

그럼에도 불구하고 태블릿과 같은 대화면 기기에서 데스크톱 환경에서만 사용할 수 있는 응용 프로그램을 사용할 수 있다는 것은 생산성 개선과 활용 측면에서 사용자에게 큰 자율성을 부여할 수 있기 때문에 그 점에서 Termux를 이용한 Linux 구동은 의의를 가진다고 판단한다. 사실 이렇게 사용자가 제조사가 원하는 의도에서 벗어나 다양한 방법으로 사용할 수 있다는 측면에서 개인적으로는 안드로이드 OS를 애플의 iOS/iPadOS보다 선호한다. 이견이 있을 수는 있겠지만, 적어도 Geek인 본인에게는 그러한 기기들이 '재미가 없다'.

다른 사용자들에게 많은 도움이 된다면 좋겠다.

## 참고자료

1. [안드로이드 태블릿에서 코딩하기 프로그래밍하기 - 안드로이드에서 우분투 리눅스로 코딩하기](https://m.blog.naver.com/einsbon/222302444210)
2. [Andronix Document](https://docs.andronix.app/)
3. [Android Phantom, Cached And Empty Processes](https://gist.github.com/agnostic-apollo/dc7e47991c512755ff26bd2d31e72ca8)
