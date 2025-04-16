
협력사 라이브러리를 gdb로 디버깅하다가..  
매번 gdb사용할 때마다 구글링해서 찾아봐서  
이번 기회에 간단한 디버깅 방법을 정리해둔다.  
## yocto에 gdb 추가
https://linux-dev.gitbook.io/linux-os-build-.../installing-gdb-in-yocto-project
~~~bash
#add the feature :-> tools-debug
EXTRA_IMAGE_FEATURES += "debug-tweaks tools-debug"
~~~

## 별도의 파일에 breakpoint 거는 방법
https://www.google.com/search?q=how+to+add+breakpoint+in+specific+file+in+gdb&oq=how+to+add+breakpoint+in+specific+file+in+gdb&gs_lcrp=EgZjaHJvbWUyCQgAEEUYORigATIHCAEQIRigATIHCAIQIRigATIHCAMQIRiPAjIHCAQQIRiPAtIBCTE0OTQ0ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8
  
https://web.eecs.umich.edu/~sugih/pointers/gdbQS.html
  
~~~
6. Setting breakpoints A breakpoint is like a stop sign in your code -- whenever gdb gets to a breakpoint it halts execution of your 
program and allows you to examine it. To set breakpoints, type "break [filename]:[linenumber]". For example, if you wanted to set a 
breakpoint at line 55 of main.cpp, you would type "break main.cpp:55".

7. Working with breakpoints
To list current breakpoints: "info break"
To delete a breakpoint: "del [breakpointnumber]"
To temporarily disable a breakpoint: "dis [breakpointnumber]"
To enable a breakpoint: "en [breakpointnumber]"
To ignore a breakpoint until it has been crossed x times:"ignore [breakpointnumber] [x]

8. Examining data When your program is stopped you can examine or set the value of any variable. To examine a variable,
 type "print [variablename]". To set the value of a variable, type "set [variablename]=[valuetoset]".
~~~

## 변수 값 보는 방법
https://cgi.cse.unsw.edu.au/~learn/debugging/modules/gdb_viewing_data/
### print
View simple data including strings, integers, structs, or a statically declared arrays (1D or 2D).
~~~
(gdb) print <variable_name>
~~~
### info locals
Print all local variables excluding the arguments passed into the function.
~~~
(gdb) info locals
~~~
### info args
Print the arguments passed into the function.
~~~
(gdb) info args
~~~

## 컴파일 & 실행
sy@sy-desktop:~/check/agilex5_res$ gcc -g check_gdb.c -o check_gdb

~~~bash
sy@sy-desktop:~/check/agilex5_res$ gdb ./check_gdb 
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./check_gdb...

# 브레이크포인트 설정
(gdb) break check_gdb.c:10
Breakpoint 1 at 0x1178: file check_gdb.c, line 10.
(gdb) break check_gdb.c:12
Breakpoint 2 at 0x11ab: file check_gdb.c, line 12.
(gdb) break check_gdb.c:17
Breakpoint 3 at 0x11e5: file check_gdb.c, line 17.
(gdb) break check_gdb.c:23
Breakpoint 4 at 0x121f: file check_gdb.c, line 23.


# 실행
(gdb) run
Starting program: /home/sy/check/agilex5_res/check_gdb 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, main () at check_gdb.c:10
~~~

## 다음 브레이크 포인트 이동, 브레이크포인트 리스트 조회
 - stepi vs nexti
https://stackoverflow.com/questions/52024529/whats-the-difference-between-nexti-and-stepi-in-gdb
  
stepi가 조금 자세하다고 한다.  


 - nexti vs next
http://maths.nju.edu.cn/help/mathhpc/doc/intel/idb/common/idb_the_next_and_nexti_commands.htm
  
next는 실행할 다음 줄에 함수 호출이 포함되어 있는 경우, 함수를 실행한다.  
nexti는 다음 기계어로 이동한다.  
  
위에서 나온 것처럼 info break하면 브레이크포인트 리스트가 출력됨


## 체크용 코드
~~~
  1 #include <stdio.h>
  2 
  3 int main()
  4 {
  5     int x = 0, y = 0, z = 0;
  6 
  7     x = 2;
  8     y = 4;
  9 
 10     z = x+y;
 11     printf("%s(), L%d: z:%d\n", __func__, __LINE__, z);
 12     z = x*y;
 13     printf("%s(), L%d: z:%d\n", __func__, __LINE__, z);
 14 
 15     x++;
 16     y++;
 17     z = x*y;
 18     printf("%s(), L%d: z:%d\n", __func__, __LINE__, z);
 19 
 20 
 21     x++;
 22     y++;
 23     z = x*y;
 24     printf("%s(), L%d: z:%d\n", __func__, __LINE__, z);
 25 
 26     return 0;
 27 }
~~~
