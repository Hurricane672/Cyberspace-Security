## strstr( )
- strstr是C语言中的函数，作用是返回字符串中首次出现子串的地址。
### 函数原型：

```c
extern char *strstr(char *str1, const char *str2);
```

### 语法：

```c
* strstr(str1,str2)
//str1: 被查找目标　string expression to search.
//str2: 要查找对象　The string expression to find.
//返回值：若str2是str1的子串，则返回str2在str1的首次出现的地址；如果str2不是str1的子串，则返回NULL。
```

### 常用实现
```c
char *strstr(const char *str1, const char *str2)
{
    char *cp = (char*)str1;
    char *s1, *s2;
 
    if (!*str2)
        return((char *)str1);
 
    while (*cp)
    {
        s1 = cp;
        s2 = (char *)str2;
 
        while (*s1 && *s2 && !(*s1 - *s2))
            s1++, s2++;
 
        if (!*s2)
            return(cp);
 
        cp++;
    }
    return(NULL);
}
```