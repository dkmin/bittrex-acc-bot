# bittrex-acc-bot
this application is 'Bittrex Accumulation Checking Bot'.

# 1. 이건 뭐하는 건가요?
이런 아이디어에서 시작했습니다.

> 누군가 코인의 가격을 올리려고 한다면 적은 거래량 와중에도 지속적으로 코인을 매집하려고 할 것이다. 이러한 움직임을 포착하려면 거래 기록을 살펴봐야겠다.

이러한 아이디어에서 착안해서 **거래량이 적은 코인들을** 모니터링 하는 프로그램을 만들어보자고 생각했고, 이 프로그램을 만들었습니다.

다만 여기서 저는 선택을 해야했습니다.

1. 거래소
2. 거래량의 기준

첫번째로 거래소 같은 경우 폴로닉스 말고 **비트렉스**를 타겟으로 했습니다. 그 이유는 폴로닉스보다는 비트렉스가 더 많은 코인을 취급하기도 하고 대체로 거래량이 적고 덜 알려진 신생 코인들을 취급하기 때문이었습니다.
두번째로 거래량이 얼마나 작은 것을 기준으로 할 것인가는 일단 24h 거래량 기준으로 **100BTC 미만**인 코인들을 모니터링 하는 것으로 했습니다. 이 기준이 적당한지는 조금 고민을 해보았지만 최적의 값이라고는 생각하지 않지만 일단 100BTC로 잡았습니다.

# 2. 어떻게 보는건가요?
접속하게되면 다음과 같은 웹 페이지를 확인할 수 있습니다.

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_1.png)

표 형태로 데이터를 제공하고 있습니다. 먼저 다음을 보시면 되겠습니다.

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_3.png)

BTC-2GIVE 의 경우 1.82% (0.01/0.46) 이라고 나와있습니다.
이 정보가 의미하는 것은 다음과 같습니다.

1. 현재로부터 1시간 이내의 거래량 중 1.82%가 Buy로 체결된 것이다. (100%라면 모든 주문이 Buy)
2. 현재로부터 1시간 이내의 거래량 중 Buy의 볼륨은 0.01BTC이다.
3. 현재로부터 1시간 이내의 총 거래량은 0.46BTC이다.

데이터가 무엇을 의미하는지 아시겠나요? 가장 위쪽에 있는 1h, 3h, 6h, 12h, 1d, 3d, 1w 등은 각각 1시간, 3시간, 6시간, 12시간, 1일, 3일, 1주일을 의미합니다.

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_4.png)

위에 있는 시간을 클릭하면 해당 열을 기준으로 데이터가 정렬됩니다.

# 3. 내가 직접 설치하여 사용하기
안타깝게도 이 프로그램은 DB에 데이터를 쌓아서 정보를 만들어내기 때문에 서버역할을 해줄 컴퓨터가 하나 필요합니다. 거래 로그를 수집해야하기 때문에 항상 켜져있는 컴퓨터가 좋습니다. 채굴이나 트레이딩을 주로 하는 분들은 분명 이렇게 사용할만한 컴퓨터가 있을거라고 생각하지만 혹여나 없으신 분들은 호스팅을 사용해서 사는 것을 추천드립니다.(트래픽이 문제가 될 것 같긴 하지만 차차 줄여보겠습니다..)

윈도우에서 설치하는 방법과 우분투에서 설치하는 방법으로 나눠서 작성하겠습니다.

## 3.1 윈도우에서 설치하기
### Node.js 설치
먼저 Node.js를 설치해야합니다. Node.js를 설치하기 위해서는 다음 링크로 들어가세요.
https://nodejs.org/ko/

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_5.png)

최신버전과 LTS 버전이 있을 텐데 여기서 LTS 버전을 선택해줍니다.
나중에는 LTS 버전도 올라가겠지만 제가 사용했던 버전은 Node 6버전입니다.
많이 사용되고 일반적인 모듈만 사용했기 때문에 나중에 의존성 문제가 발생할 여지는 적다고 생각합니다..(그래도 혹시 발생하면 이슈 남겨주세요.)

다운로드가 완료되었다면 실행시켜서 쭉 Next, Install 눌러주시면 됩니다.
설치가 완료되었다면 Finish를 눌러주시고 왼쪽 아래의 시작버튼을 눌러서 cmd라고 검색하고 실행을 합니다.

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_6.png)

거기서 node --version 이라고 입력했을 때 자신이 설치한 버전의 Node.js가 나온다면 정상적으로 설치가 된겁니다.

### Mysql 설치
Mysql은 다음 링크를 클릭하여 다운로드 할 수 있습니다.
https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-web-community-5.7.18.1.msi
여기서 받아지는 것은 Mysql 본 파일이 아닌 인스톨러 입니다. 이 인스톨러를 실행시킵니다.
이때! 설치가 잘 될 수도 있겠지만 다음과 같은 에러를 만나실 수도 있습니다. (문제없이 쭉 진행되시는 분은 건너 뛰셔도 됩니다.)

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_7.png)

이를 해결하려면 다음 링크로 이동하세요.
https://www.microsoft.com/ko-KR/download/details.aspx?id=17113
다운로드를 클릭 후 다음 화면으로 넘어가면 체크박스에 있는 것들은 체크하지 말고 오른쪽 아래의 [건너뛰고 다음 단계 진행]을 클릭합니다.
**dotNetFx40_Client_setup.exe** 파일이 다운로드 되었을 것입니다. 실행시켜줍니다. 다음 쭉 눌러주시면 설치가 될겁니다.

다시 Mysql 설치로 돌아와서, 인스톨러를 실행시키고 다음을 누르면 아래와 같은 선택지를 만나게됩니다. 여기서부턴 쭉 따라하시면됩니다.

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_8.png)
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_9.png)
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_10.png)
비슷한게 몇번 더 나올겁니다. 같은 방식으로 약관에 동의 체크, 다음을 눌러주시면 됩니다.
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_11.png)
복구와 제거 중에 선택하는 것에서는 복구를 선택해주세요.
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_12.png)
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_13.png)
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_14.png)
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_15.png)
Error 혹은 Failed가 떠도 걱정마세요. Server만 정상적으로 설치되면 됩니다.
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_16.png)
쭉 Next를 눌러서 다음 화면까지 오세요.
![alt text](https://github.com/lleellee0/images/blob/master/screenshot_17.png)
기존에 Mysql을 설치하지 않은 분이라면 3306으로 알아서 입력이 되어있겠지만 혹 Mysql을 두번째 설치하시는 분이라면 다음 포트가 잡혀있을 겁니다. 기존에 Mysql을 설치해보셨던 분이라면 어떻게 해야할지 알고 계실겁니다. :)

DB 관련 설정 변경

![alt text](https://github.com/lleellee0/images/blob/master/screenshot_2.png)

## 3.2 우분투에서 설치하기

# 4. 개발하면서 느낀점
