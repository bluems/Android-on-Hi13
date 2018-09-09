CHUWI Hi13에 Android 설치 방법
====================================
이 리포지토리는 CHUWI Hi13 모델에 Android를 설치하기 위해 작성되었습니다. <br>
안드로이드를 설치하기 위해 forum.chuwi.com 과 techtablets.com 등을 열심히 구글링했지만 대부분의 글이 다음과 같은 포스팅이었습니다.

* Hi13에 안드로이드를 설치했는데 터치가 먹히지 않습니다.
* Hi13에 안드로이드를 어떻게 설치하나요?
* Hi13에 우분투와 함께 안드로이드를 설치하는 방법

왜 우분투를 설치해야 하나요? <br>
우분투는 필수 사항이 아닐 뿐더러 안그래도 적은 용량에 다른 리눅스까지 할애할 수는 없습니다. <br>
그렇다고 sdcard에 설치하여 슬롯을 낭비하는 것도 어리석은 방법입니다. <br>
그래서 저는 EFI 부트 방법과 rEFInd에 대해 조사하였고 다른 리눅스나 OS의 도움 없이 순수 안드로이드만 설치하는 방법을 찾았습니다. <br><br>
먼저, 설치 방법에 대해 기술하기 전에 이 방법을 찾을 수 있게끔 도와주신 이 모 AMI Inc. 직원분과 우분투와 함께 설치하는 방법을 알려준 Joost van der Wulp 에게 감사를 드립니다. <br>

저는 백승현 이고 군산대학교에서 석사과정으로 지내고 있습니다.(깨알 어필 맞습니다)

## Partitioning
두 가지 방법이 모두 가능합니다.
기존 윈도우를 살리고 싶다면 C의 용량을 줄이고 진행하시면 됩니다만 이 리포에서는 안드로이드만 설명합니다.

### First Solution: 기존 파티션 재활용

mmcblk1p1 은 Hi13 F/W의 기본 부트로더 파티션입니다.

```
mmcblk1p1 => EFI System
mmcblk1p2 => Windows RE
mmcblk1p3 => Windows
....
```

안드로이드를 설치할 때 cfdisk에서 아래와 같이 1p1만 남기고 삭제하시면 됩니다.
```
Delete: mmcblk1p2, mmcblk1p3. not mmcblk1p1
Create: 아마 새로만든 파티션은 mmcblk1p2 또는 1p3 일겁니다. 볼륨 레이블은 원하시는 것으로 하세요.
```

### Second Solution: 모든 파티션 삭제 후 새로 작성하는 방법.
모든 파티션을 제거하고 새로 설치하고 싶으시다면 1번째 파티션을 fat32로 만드시면 됩니다.
```
Delete: all
Create: mmcblk1p1 => 100MB(권장)
        mmcblk1p2 => All
```
그리고 아래 가이드라인을 따라주세요.

## Install

### First, 안드로이드 설치 및 Ubuntu 라이브 부팅.
이 [포스팅](http://chuwi-hi13-install-ubuntu.blogspot.com/2017/06/how-to-install-android-on-chuwi-hi13.html)을 따라 진행하면 됩니다

### Second, 아래 명령어를 입력하세요.
터미널은 좌상단 케노니컬 로고를 눌러 검색하면 됩니다.
```
sudo mount /dev/mmcblk1p1 /mnt
cd /mnt
cp -R This-repository-files .
```
간단히 rEFInd를 설치했습니다.

### Configure: refind.conf
```
nano refind.conf
```
파일 끝으로 가서 아래 내용을 추가해주세요.
```
menuentry "Android 7.1" {
    volume "your volume label name"
    ...
}

```

네? 설마설마 직접 지은 레이블을 까먹었어요? 
걱정하지 않아도 됩니다. 찾을 수 있거든요.
```
e2label /dev/<your-android-block>
```
아니, 이번엔 갑자기 맘에 안드시나요?
걱정마세요. 바꾸면 되죠.
```
e2label /dev/<your-android-block> new-name
```

## Finally
어서 설치한 안드로이드로 돌아가세요.
설치를 마쳤으면 돌아가야죠 :)

## 주의사항
제가 테스트한 Hi13은 Goodix의 gdix1001 터치를 사용합니다.
아마 17070001을 포함한 이후 모델은 gdix1002를 사용하는 것으로 생각되는데 안타깝게도 저는 그 터치 모델은 테스트를 할 수가 없습니다.
대신 gdix1002 드라이버를 추가할 수 있는 이 [쓰레드](https://techtablets.com/forum/topic/updating-bios-and-installing-linux/)가 도움이 되었으면 합니다.
