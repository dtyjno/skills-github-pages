---
title: Welcome to my blog
---

#### Support ***regular expressions*** in name matching for find. grep.c has some primitive support for regular expressions. (easy)

在 find 程序中支持正则表达式匹配文件名。grep.c 中有一些基本的正则表达式支持，可以参考它来实现。

grep.c
```c
// Regexp matcher from Kernighan & Pike,
// The Practice of Programming, Chapter 9.
//来自Kernighan & Pike的Regexp匹配器，
//《编程实践》，第9章。

int matchhere(char*, char*);
int matchstar(int, char*, char*);

int
match(char *re, char *text)
{
  if(re[0] == '^')
    return matchhere(re+1, text);
  do{  // must look at empty string
    if(matchhere(re, text))
      return 1;
  }while(*text++ != '\0');
  return 0;
}

// matchhere: search for re at beginning of text
int matchhere(char *re, char *text)
{
  if(re[0] == '\0')
    return 1;
  if(re[1] == '*')
    return matchstar(re[0], re+2, text);
  if(re[0] == '$' && re[1] == '\0')
    return *text == '\0';
  if(*text!='\0' && (re[0]=='.' || re[0]==*text))
    return matchhere(re+1, text+1);
  return 0;
}

// matchstar: search for c*re at beginning of text
int matchstar(int c, char *re, char *text) 
{
  do { // 循环尝试匹配0次或多次c
    if(matchhere(re, text)) // 先尝试匹配0次（跳过c*）
      return 1;             
  } while(*text!='\0' && (*text++==c || c=='.')); 
  // 若匹配1次c（或c是.），移动text指针继续
  return 0;
}

```
