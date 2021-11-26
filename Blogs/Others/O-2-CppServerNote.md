# cpp server 
## contents
### 1. Libs
1.1 time.h 


# 1. Libs
## 1.1 Lib time.h
Unix时间戳，定义为由1970年至今的总秒数。

[https://www.runoob.com/cprogramming/c-standard-library-time-h.html]

结构体有：
```
struct timeval{
    long tv_sec;
    long tv_usec;
}
```

```
struct tm{
    int tm_sec; // seconds
    int tm_min; // mins
    int tm_hour; // hours
    int tm_mday; // day in a month
    int tm_mon; // month
    int tm_year; // year
    int tm_wday; // day in a week
    int tm_yday; // day in a year
    int tm_isdst;
}
```
常用函数：
```
char* asctime(tm*);
clock_t clock(void);
tm* localtime(time);
size_t strftime(char *str, size_t maxsize, const char *format, const struct tm *timeptr)
```


## boost
