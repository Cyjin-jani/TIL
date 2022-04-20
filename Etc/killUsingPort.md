## 사용중인 port 죽이기
<br>
어떠한 연유로 인해, 분명 npm run dev를 종료시켰음에도,
다시 키려고 하면 해당 포트는 사용중이다라는 에러가 나면서 프로젝트가 켜지지 않는 버그(?)가 발생했다.

이러한 경우에는 실제 port의 상태를 확인한 뒤, 강제종료를 시킬 수 있다.

상태를 확인하는 방법은 아래 명령어를 입력하면 된다.

```tsx
lsof -i :[portNumber]

// ex)
lsof -i :4001 //포트번호
```

그러자 아래와 같은 상태가 나왔다.

```tsx
COMMAND  PID           USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node    3751 corretto-dev-1   23u  IPv6 0x88f9a60b6a8adbd5      0t0  TCP *:newoak (LISTEN)
node    3751 corretto-dev-1   29u  IPv6 0x88f9a60b703e6235      0t0  TCP localhost:newoak->localhost:51454 (CLOSE_WAIT)
node    3751 corretto-dev-1   30u  IPv6 0x88f9a60b703e48b5      0t0  TCP localhost:newoak->localhost:51455 (CLOSE_WAIT)
node    3751 corretto-dev-1   32u  IPv6 0x88f9a60b703e8215      0t0  TCP localhost:newoak->localhost:51456 (CLOSE_WAIT)
node    3751 corretto-dev-1   33u  IPv6 0x88f9a60b70403555      0t0  TCP localhost:newoak->localhost:51457 (CLOSE_WAIT)
node    3751 corretto-dev-1   34u  IPv6 0x88f9a60b70404215      0t0  TCP localhost:newoak->localhost:51458 (CLOSE_WAIT)
node    3751 corretto-dev-1   35u  IPv6 0x88f9a60b704008b5      0t0  TCP localhost:newoak->localhost:51459 (CLOSE_WAIT)
node    3751 corretto-dev-1   36u  IPv6 0x88f9a60b6b4348b5      0t0  TCP localhost:newoak->localhost:51238 (CLOSE_WAIT)
node    3751 corretto-dev-1   68u  IPv6 0x88f9a60b703e7bb5      0t0  TCP localhost:newoak->localhost:51201 (CLOSE_WAIT)
```

close-wait가 걸려있는걸로 봐서, 역시 종료가 제대로 되지 않았다고 본다.

그래서 강제종료를 해주어야 하는데, 즉시 강제종료 하는 명령어는 아래와 같다.

```tsx
kill -9 [PID]

// ex)
kill -9 3751
```

kill할 때에는 port의 번호가 아니라 PID를 넣어주어야 한다! 그렇기에 위에서 해당 포트의 상태를 보는 건 필수적인 행동이다.

*참고*

-9는 사실 즉시 종료이기 때문에, 제대로 cleanup되지 않을 수 있다.

그래서 -15 혹은 -3을 먼저 시도해 보는 것이 좋다.

[https://stackoverflow.com/questions/3855127/find-and-kill-process-locking-port-3000-on-mac](https://stackoverflow.com/questions/3855127/find-and-kill-process-locking-port-3000-on-mac)