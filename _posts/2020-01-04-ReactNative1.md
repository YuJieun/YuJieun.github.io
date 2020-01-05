---
title:  "[Nomad Coders - React Native (1) - 준비편]"
date:   2020-01-04
tags: [Javascript, ReactNative]
toc: true
---

[Nomad Coders - React Native (1) - 준비편]



### 1. 사전 준비사항(환경 만들기)

- node.js설치하기

  Homebrew를 이용해서 node.js를 설치하여 버전관리 등등 좋다고 한다. 그래서 다음순으로 설치를 하였다.

  - Homebrew설치

    Homebrew는 맥에서 프로그램을 쉽게 다운로드 및 삭제할 수 있는 패키지 관리자이다. 

    터미널에서  다음 명령어로 설치함

    ```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

  - Cask 설치

    Cask는 일반적으로 쓰는 GUI 기반의 어플리케이션(크롬 등)을 설치하기 위함이다.

    `brew install cask` 명령어로 설치함

  - nvm 설치

    nvm이란 Node Version Manager의 약자. 여러 노드 버전을 사용해야할 때 유용.

    `brew install nvm` 으로 설치 후, `mkdir ~/.nvm` 으로 디렉토리 생성

    ~/.bash_profile파일을 vi에디터로 연 후
    
    ```
    export NVM_DIR="$HOME/.nvm"
    [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
    [ -s "/usr/local/opt/nvm/etc/bash_completion" ] && . "/usr/local/opt/nvm/etc/bash_completion"  # This loads nvm bash_completion
    ```
    
    작성 후 저장.
    
    source ~/.bash_profile 입력후 nvm명령어가 먹는지 확인! (nvm --version)
    
  - Node.js설치
  
    `nvm install stable` 로 노드 최신버전 설치
  
    만약, 여러 버전의 노드를 설치한 경우 `nvm use v4.2.6` 과 같은 형태로 전환가능.
  
  - iterm2설치
  
    이거는 강의속에서 iterm2라는 터미널 비슷한게 보여서 무작정 설치했는데, 맥의 터미널을 커스터마이징 하기 위한 것 같다. 이건 미리 설치해두었으니 다음번에 iTerm2와 zsh로 터미널을 변경해보아겠다.
  
  - Expo client
  
    프로젝트를 핸드폰에서 돌리기 위해, Expo client앱을 설치함. 
  
    또한, `npm install -g expo-cli` 로 터미널에서 설치.
  
    

### 2. What is Expo?

**expo는 create-react-app**. 리엑트 네이티브를 위한 설정 파일같은 것들이 없는 방식으로, 모든것이 set up 되어있음.

1) **React native CLI 방식** native files들을 많이 컨트롤 하고 싶을때를 위함. 

xcode와 안드로이드스튜디오가 파일 생성. 양쪽 파일들이 리엑트 네이티브에서 실행됨. 양쪽 파일에 접근 가능하며, 어떤 때는 둘을 결합할 수 있음. native파일에 접근을 하는것임.

2) **expo방식**. 모든 native files들을 숨기고, 모든것을 관리해줌. 

가장 큰 장점은, 내 휴대폰에서 앱을 테스트할 수 있다는 점. 만약에 react native cli방식으로 앱 테스트 하려면 개발자 계정 등등, xcode connect등에 로그인해야되고 아주 귀찮음. 단점은 native files에 접근을 하지 못한다는 정도

수동적 작업들이 덜 필요하고, 시작하는데 훨씬 더 빠른 방식임. 하지만 native파일에 접근하지 못하므로 한계가 있긴 함!



### 3. Create Expo Project

fokin-weather라는 프로젝트 생성함

app.json --> configuration파일 (expo가 읽게 됨)

------

`npm start`라고 입력하여 expo 애플리케이션 시작

자동으로 expo DEV tool 오픈됨.

큐알코드 -->  안드로이드 폰이 스캔. 프로젝트 열고 앱을 폰에서 테스트해볼 수 있음. ios는 큐알코드가 없으므로 앱에서 로그인 후, pc 콘솔창에서 expo login입력

------

폰과 컴이 같은 와이파이에 연결되어야함

새로고침은 command+R

폰에서 개발자 모드 열고 싶으면, 폰을 흔들면 됨.

------

debug remote.js  어플리케이션 디버그 하는 것을 보여줌. 앱의 로그를 볼 수 있음(크롬에서)

이거는 좀 느리므로 필요한 경우에만 쓰기. 네트워크 요청, 콘솔, 크롬작업 등등...

------



### 4. React Native

1. 모바일 앱을 만드는 방법

   - 완전 native 

     swift or objective-c로 ios앱 만들고, java or kotlin으로 안드로이드 앱 만드는 것.  프로그래밍 언어도 다르고 매우 다름. Xcode와 Android Studio이용.

   - 웹 기반 웹뷰

     매우매우 심플한 앱 만들 때. 앱 안에서 작동하는 웹사이트 같은 것. 하이브리드,웹뷰 방식. 껍데기가 있고, 앱을 오픈할때 보는 것은 html, css, javascript로 만들어진 웹사이트같은 것. 전체 웹사이트를 앱안에다 넣는것임

   - React Native

     Android,ios는 둘다 자바스크립트 실행 가능. (자바스크립트 엔진이 있기 때문). 리엑트 네이티브는 자바스크립트를 이용해서 ios또는 android 네이티브 엔진에게 자바스크립트 메세지를 보내는 것임. 

     브릿지(자바스크립트 - 안드로이드, ios 네이티브 텍스트, 뷰를 이어줌)

     컴포넌트가 있고(자바스크립트) 컴포넌트 안에 브릿지가 있음. 우리는 자바스크립트와 아오스,안드 사이에서 서로를 이해하기 위한 사이의 브릿지를 만들어야함. 

     이게 느린 성능을 유발할 수도 있다. 브릿지로 많은 데이터를 보내면, 교통체증처럼 부하가 걸리니까...

     어플만들기에는 굳인데, 만약에 3D비디오 게임 같은 것을 만드려면 리액트가 최선은 아님. 코드를 최적화 하는데 많은 시간을 써야 하므로.(브릿지가 느려지지 않도록)

2. 리엑트 네이티브 - 컴포넌트

   리엑트 세상에서는 모든게 view이다. 전부다 view이고 view안에 모든것을 집어넣어야함.

   ```javascript
   <View style={styles.container}>
     <Text>Hello!!</Text>
   </View>
   ```

   StyleSheet API로 stylesheet생성. 

3.  connection reset 오류 --> 재시작하기(npm)

4. ```javascript
   <View style={styles.container}>
     <Text style={styles.text}>Hello!!</Text>
   </View>
   
   const styles = StyleSheet.create({
     container: {
       flex: 1,
       backgroundColor: '#fff',
       alignItems: 'center',
       justifyContent: 'center',
     },
     text: {
       color: "white"
     }
   });
   
   ```

   텍스트의 색상을 바꾸고 싶을때.

   

