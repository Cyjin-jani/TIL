## 크로미움 기반 브라우저에서 막혀있는 포트
<br>
포트번호를 3000번대에서 사용하던 중, 다른 여러가지 이유로 서버 포트번호를 6000으로 바꾸었는데,

unsafe-port 라는 에러가 발생했다.

찾아보니, chromium 기반의 브라우저에서는 6000번을 안전하지 않은 포트 넘버로 분류했고,

그로 인해 제대로 된 api 통신이 되지 않았던 것이다.

사용할 수 없는 포트 넘버 리스트는 아래와 같다.

```jsx
		1,      // tcpmux
    7,      // echo
    9,      // discard
    11,     // systat
    13,     // daytime
    15,     // netstat
    17,     // qotd
    19,     // chargen
    20,     // ftp data
    21,     // ftp access
    22,     // ssh
    23,     // telnet
    25,     // smtp
    37,     // time
    42,     // name
    43,     // nicname
    53,     // domain
    69,     // tftp
    77,     // priv-rjs
    79,     // finger
    87,     // ttylink
    95,     // supdup
    101,    // hostriame
    102,    // iso-tsap
    103,    // gppitnp
    104,    // acr-nema
    109,    // pop2
    110,    // pop3
    111,    // sunrpc
    113,    // auth
    115,    // sftp
    117,    // uucp-path
    119,    // nntp
    123,    // NTP
    135,    // loc-srv /epmap
    137,    // netbios
    139,    // netbios
    143,    // imap2
    161,    // snmp
    179,    // BGP
    389,    // ldap
    427,    // SLP (Also used by Apple Filing Protocol)
    465,    // smtp+ssl
    512,    // print / exec
    513,    // login
    514,    // shell
    515,    // printer
    526,    // tempo
    530,    // courier
    531,    // chat
    532,    // netnews
    540,    // uucp
    548,    // AFP (Apple Filing Protocol)
    554,    // rtsp
    556,    // remotefs
    563,    // nntp+ssl
    587,    // smtp (rfc6409)
    601,    // syslog-conn (rfc3195)
    636,    // ldap+ssl
    989,    // ftps-data
    990,    // ftps
    993,    // ldap+ssl
    995,    // pop3+ssl
    1719,   // h323gatestat
    1720,   // h323hostcall
    1723,   // pptp
    2049,   // nfs
    3659,   // apple-sasl / PasswordServer
    4045,   // lockd
    5060,   // sip
    5061,   // sips
    6000,   // X11
    6566,   // sane-port
    6665,   // Alternate IRC [Apple addition]
    6666,   // Alternate IRC [Apple addition]
    6667,   // Standard IRC [Apple addition]
    6668,   // Alternate IRC [Apple addition]
    6669,   // Alternate IRC [Apple addition]
    6697,   // IRC + TLS
    10080,  // Amanda
```

[https://chromium.googlesource.com/chromium/src.git/+/refs/heads/master/net/base/port_util.cc](https://chromium.googlesource.com/chromium/src.git/+/refs/heads/master/net/base/port_util.cc)