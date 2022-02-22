---
title: 透過 RS232 進行電腦訊息互傳
date: 2017-06-09 14:47:40
draft: false
tags: ["RS232"]
categories:  [學習札記, 專題]
---

禮拜一花了 9 個小時
完成了電腦之間用 RS232 互傳
以及電腦對字幕機的寫入

## 需要器材
1. 兩條 USB 轉 RS232接線
![](http://i.imgur.com/zvxP8qr.jpg)
2. RS232 兩端母對母的線
![](http://i.imgur.com/99xerSB.jpg)
母對母的需要對TX, RX做跳線(外面賣的是沒有跳線)

## 電腦之間互傳
首先是我們在網路上找到的 RS232 C Library
[RS232 下載](http://www.teuniz.net/RS-232/)

打開下載回來的 RS232 包
![](http://i.imgur.com/0HLzL9Z.png)
可以看到裡面有 demo_tx.c 和 demo_rx.c
TX, RX 分別代表傳輸和接收
## TX
要修改的是 cport_nr 這個變數
他代表要用哪個連接埠傳輸, 可以到裝置管理員看是哪一個
我們的是 COM4 所以改成 cport_nr=3
然後 bdrate 代表的是 baud rate, 是傳輸速率
```c
/**************************************************

file: demo_tx.c
purpose: simple demo that transmits characters to
the serial port and print them on the screen,
exit the program by pressing Ctrl-C

compile with the command: gcc demo_tx.c rs232.c -Wall -Wextra -o2 -o test_tx

**************************************************/

#include <stdlib.h>
#include <stdio.h>

#ifdef _WIN32
#include <Windows.h>
#else
#include <unistd.h>
#endif

#include "rs232.h"

int main()
{
  int i=0,
      cport_nr=3,        /* /dev/ttyS0 (COM1 on windows) */
      bdrate=9600;       /* 9600 baud */

  char mode[]={'8','N','1',0},
       str[2][512];


  strcpy(str[0], "The quick brown fox jumped over the lazy grey dog.\n");

  strcpy(str[1], "Happy serial programming!\n");

  if(RS232_OpenComport(cport_nr, bdrate, mode))
  {
    printf("Can not open comport\n");

    return(0);
  }

  while(1)
  {
    char s[100];
    scanf("%s", s);
    RS232_cputs(cport_nr, s);

    printf("sent: %s\n", s);

#ifdef _WIN32
    Sleep(1000);
#else
    usleep(1000000);  /* sleep for 1 Second */
#endif

    // i++;
    //
    // i %= 2;
  }

  return(0);
}
```

## RX
一樣修改好連接埠

```c

/**************************************************

file: demo_rx.c
purpose: simple demo that receives characters from
the serial port and print them on the screen,
exit the program by pressing Ctrl-C

compile with the command: gcc demo_rx.c rs232.c -Wall -Wextra -o2 -o test_rx

**************************************************/

#include <stdlib.h>
#include <stdio.h>

#ifdef _WIN32
#include <Windows.h>
#else
#include <unistd.h>
#endif

#include "rs232.h"

int main()
{
  int i, n,
      cport_nr=3,        /* /dev/ttyS0 (COM1 on windows) */
      bdrate=9600;       /* 9600 baud */

  unsigned char buf[4096];

  char mode[]={'8','N','1',0};


  if(RS232_OpenComport(cport_nr, bdrate, mode))
  {
    printf("Can not open comport\n");

    return(0);
  }

  while(1)
  {
    n = RS232_PollComport(cport_nr, buf, 4095);

    if(n > 0)
    {
      buf[n] = 0;   /* always put a "null" at the end of a string! */

      for(i=0; i < n; i++)
      {
        if(buf[i] < 32)  /* replace unreadable control-codes by dots */
        {
          buf[i] = '.';
        }
      }

      printf("received %i bytes: %s\n", n, (char *)buf);
    }

#ifdef _WIN32
    Sleep(100);
#else
    usleep(100000);  /* sleep for 100 milliSeconds */
#endif
  }

  return(0);
}

```

## 傳輸
接著把兩個程式碼編譯好之後
一台電腦執行 TX, 負責傳輸
一台電腦執行 RX, 負責接收
