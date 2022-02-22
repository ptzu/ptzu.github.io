---
title: Compiler(一)：環境建制以及Lexer
date: 2017-04-03 22:32:26
draft: false
tags: ["Codeforces", "Compiler"]
categories: [學習札記, 編譯器]
---
使用 ANTLR V3
[ANTLR官網](http://www.antlr.org/)

<img src = http://i.imgur.com/ETbHMma.png width="50%" height="50%">

<img src = http://i.imgur.com/69XBnEu.pngg width="50%" height="50%">

<img src = http://i.imgur.com/cjxZADZ.png width="50%" height="50%">

ANTLR 利用 .g 檔生成 Lexer
定義 Token 如下:

```
INT_TYPE  : 'int';
LONGLONG_TYPE : 'long long';
CHAR_TYPE : 'char';
VOID_TYPE : 'void';
FLOAT_TYPE: 'float';
WHILE_    : 'while';
FOR_ : 'for';
IF_ : 'if';
ELSE_ : 'else';
```

一些常見型態如: int, long long, char, void, float
一些保留字如: while, for, if, else

```
ASSIGN_OP : '=';
P_OP : '+';
M_OP : '-';
MU_OP : '*';
MUA_OP : '*=';
D_OP : '/';
DA_OP : '/=';
PA_OP : '+=';
MA_OP : '-=';
EQ_OP : '==';
LE_OP : '<=';
GE_OP : '>=';
NE_OP : '!=';
PP_OP : '++';
MM_OP : '--';
RSHIFT_OP : '<<';
LSHIFT_OP : '>>';
```

各種運算子:
令值: '='
加減乘除: '+, -, *, /'
位元運算: '<<', '>>'

```
PUNCTUATION : ('.'|','|';'|'#'|'('|')'|'{'|'}'|'<'|'>'|'%'|'\\') ;
```
標點符號: '. , ; # ( ) { } < > % \'

```
DEC_NUM : ('0' | ('1'..'9')(DIGIT)*) ;
```
十進位數字

```
ID : (LETTER)(LETTER | DIGIT)*;
```
變數或函數名稱

```
LITERAL : ('"')(LETTER|PUNCTUATION|NEW_LINE|' ')*('"') ;
```
字串


```
REFERENCE : ('&')(ID);
```
變數記憶體位址

```
FLOAT_NUM: FLOAT_NUM1 | FLOAT_NUM2 | FLOAT_NUM3;
```
浮點數

```
COMMENT1 : '//'(.)*'\n';
COMMENT2 : '/*' (options{greedy=false;}: .)* '*/';
```
註解

```
NEW_LINE: ('\n'|'\r');
```
換行字元

fragment: 非一個 token, 可以被別人引用

```
fragment FLOAT_NUM1: (DIGIT)+'.'(DIGIT)*;
fragment FLOAT_NUM2: '.'(DIGIT)+;
fragment FLOAT_NUM3: (DIGIT)+;
```
各種浮點數樣式

```
fragment LETTER : 'a'..'z' | 'A'..'Z' | '_';
字母
fragment DIGIT : '0'..'9';
數字

WS  : (' '|'\r'|'\t')+
    ;
空白
```

指令： 透過 MyLexer.g 生成 MyLexer.java
```
java -cp antlr-3.5.2-complete.jar org.antlr.Tool MyLexer.g
```

指令： 將你要呼叫 Lexer 的檔案和 Lexer 一起編譯
```
javac -cp ./antlr-3.5.2-complete.jar TestLexer.java MyLexer.java
```

指令： 執行, 並將要測試的檔名用 args 傳入
```
java -cp .:antlr-3.5.2-complete.jar TestLexer input.c
```

其中 -cp 代表的是 -classpath, 告訴編譯器 class 的路徑
.:antlr-3.5.2-complete.jar
意思是 . 和 antlr-3.5.2-complete.jar 這兩個路徑
以 : 隔開, 在 windows 是以 ; 隔開
