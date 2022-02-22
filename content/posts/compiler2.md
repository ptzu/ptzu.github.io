---
title: compiler2
date: 2017-05-12 12:00:33
draft: false
tags: ["Codeforces", "Compiler"]
categories:
---
java -cp antlr-3.5.2-complete.jar org.antlr.Tool myC.g

javac -cp ./antlr-3.5.2-complete.jar:. myC_test.java

java -cp .:antlr-3.5.2-complete.jar myC_test input1.c
