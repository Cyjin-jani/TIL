## vscode에서 code path 설정하기
<br>

터미널에서 code . 라는 명령어를 이용해서 vscode 코드 편집기로 간편하게 코드를 보려는 설정을 진행하였으나, 다음과 같은 에러가 발생하였다…

*에러상황*

다음과 같은 permission error가 발생한다.

EACCES: permission denied, unlink 'usr/local/bin/code'

*해결법

bin 디렉토리에 접근하여

PATH code 를 지우고 나서 다시 'install code'  shell command를 입력하여 해결하였다.

```bash
cd /usr/local/bin
sudo rm -rf code

//in VScode
//command+shift+p 클릭 후 아래 커맨드 입력
install code
```

아마 설정하는 과정에서 무언가 잘못 세팅되었던 듯 하다.

위와 같은 방법을 사용하고나니 무난하게 사용이 가능해졌다.

참고

[https://jemerald.tistory.com/97](https://jemerald.tistory.com/97)