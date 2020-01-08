---
title:  "MAC 맥 터미널 꾸미기"
date:   2020-01-06
tags: [Something]
toc: true
---

1. iTerm2 설치

2. zsh설치

   ```
   bres install zsh
   ```

3. zsh의 플러그인 oh-my-zsh설치(zsh을 좀 더 편리하게 이용하게 해주는 일종의 플러그인)

   ```
   sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
   ```

4. Oceanic-next-item 터미널 컬러 다운 및 preference 메뉴를 통해 색상 적용

5. zsh테마를 스크린샷처럼 Agnoster 테마로 변경.

   - ~/.zshrc파일 vi편집기로 열고 ZSH_THEME="agnoster"로 수정.

6. 테마가 변경되면 폰트가 일부 꺠지므로, Ubuntu_Mono_derivative_Powerline.ttf다운받아 서체 설치 및 적용

7. 시스템의 PATH에 등록된 명령어들을 자동 하이라이팅 해주는 zsh-syntax-highlighting섪치

8. 완료

   ![terminal](./assets/images/terminal.png)

