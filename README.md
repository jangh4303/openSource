# openSource project #1
*** 
**To Do List**

* getopt 명령어 조사
* getopts 명령어로의 발전 조사
* sed 명령어 조사
* awk 명령어 조사


>  ------------
>  getopt 명령어
>  ------------

getopt 용도
  * 명령행 플래그와 매개변수를 구문 분석

getopt를 사용하는 이유

1) 다양한 입력 값이 존재할 경우 사용자와 개발자의 편의를 보장하기 위함
2) 스크립트를 보다 체계적으로 관리할 수 있음

설명 

getopt 명령은 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석합니다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 합니다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없습니다.

getopt 명령은 **모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이** 발생하는 경우 처리를 완료합니다. 그러면 getopt 명령은 처리된 플래그, -(더블 하이픈) 및 남아 있는 토큰을 출력합니다.

다음 과 같이 getopt 명령을 쉘 스크립트에서 사용하여 옵션을 구문 분석할 수 있습니다.

```
#!/usr/bin/bsh
# parse command line into arguments
set -- `getopt a:bc $*`
# check result of parsing
if [ $? != 0 ]
then
        exit 1
fi
while [ $1 != -- ]
do
        case $1 in
        -a)     # set up the -a flag
                AFLG=1
                AARG=$2
                shift;;
        -b)     # set up the -b flag
                BFLG=1;;
        -c)     # set up the -c flag
                CFLG=1;;
        esac
        shift   # next flag
done
```

getopts 명령의 경우 옵션들 중간에 파일명이 온다거나 하면 이후의 옵션은 옵션으로 인식이 되지 않는데 getopt 명령의 경우는 올바르게 구분하여 정렬해 줍니다.

```
#!/bin/bash

while getopts "a:bc" opt; do
  case $opt in
    a)
      echo >&2 "-a was triggered!, OPTARG: $OPTARG"
      ;;
    b)
      echo >&2 "-b was triggered!"
      ;;
    c)
      echo >&2 "-c was triggered!"
      ;;
  esac
done

shift $(( OPTIND - 1 ))
echo ------------------
echo "$@"
```
* getopts

```
$ ./test.sh -a 123 hello.c -bc
-a was triggered!, OPTARG: 123
------------------
hello.c -bc
```
* getopt
```
#!/bin/bash

options=$(getopt -o a:bc -- "$@")
echo $options
.................................

$ ./test.sh -a 123 hello.c -bc
-a '123' -b -c -- 'hello.c'
```

>  ------------
>  getops 명령어
>  ------------

![image](https://user-images.githubusercontent.com/94778069/142756507-03b390a8-2009-4134-a8b0-3eb744bbb895.png)

쉘 에서 명령을 실행할 때 옵션을 사용하는데요. 스크립트 파일이나 함수를 실행할 때도 동일하게 옵션을 사용할 수 있습니다. 사용된 옵션은 다른 인수들과 마찬가지로 $1, $2, ... positional parameters 형태로 전달되므로 스크립트 내에서 직접 옵션을 해석해서 사용해야 됩니다. 이때 옵션 해석 작업을 쉽게 도와주는 명령이 getopts 입니다.

옵션에는 short 옵션과 long 옵션이 있는데 getopts 명령은 short 옵션을 처리합니다.

|short 옵션|long 옵션|
|:---|:---|
|short 옵션은 여러 가지 방법으로 사용할 수 있습니다. 그러므로 getopts 명령을 이용하지 않고 직접 옵션을 해석해 처리한다면 옵션 처리에만 스크립트가 복잡해질 수 있습니다.|--posix, --warning level 와 같은 형태로 사용되는 long 옵션은 short 옵션과는 달리 붙여 쓸 수가 없기 때문에 사용방법이 간단하여 직접 해석해서 처리하는 것이 어렵지 않습니다.|

>  
> sed 명령어
>  

설명 

* ed명령어와 grep명령어 기능의 일부를 합친 것이 sed(stream editor)명령어 이다.
* sed명령어는 1개 라인씩 입력 라인을 읽어들여 표준출력으로 출력한다.
* 일치하는 문자열이 있으면 그 문자열을 대치한 후 출력하고 일치하는 문자열이 없으면 그 라인은 수정되지 않고 그대로 출력된다.

**sed 명령어 사용법**

<test 파일>

![image](https://user-images.githubusercontent.com/94778069/142757268-d8c13ce7-de5c-45f1-acb0-1731a4463619.png)


 * 치환 
    
    
    예를들어 test파일의 added 를 add로 변경하여 출력 
    `sed 's/added/add/' test`

 ![image](https://user-images.githubusercontent.com/94778069/142757295-50f87229-2df3-472f-b673-245cee09c2be.png)
 
 * 삭제

    test파일의 line이 들어간 줄을 삭제하여 출력 
    `sed '/line/d' test`
    
![image](https://user-images.githubusercontent.com/94778069/142757410-5d639956-9bd0-4a0f-b247-16a86b1876cc.png)

이외에도 다양한 사용 방법들이 있다.


>  ------------
>  awk 명령어
>  ------------

설명

* 표준입력으로 값을 받아 awk 스크립트를 통해 원하는 표준출력을 내보낼 수 있다.
* awk는 라인을 받아서 구분자를 통해 구분하고 print 명령으로 출력하게 된다.


사용 방법

![image](https://user-images.githubusercontent.com/94778069/142757599-94dfbd2a-fc25-456e-8e68-9d23917e18ef.png)

* -F : 구분자를 나타낸다. -F로 구분자를 지정하지 않을 경우에는 공백을 구분자로 사용한다.
* -f : 스크립트 파일을 이용할 경우 사용한다.

awk를 이용하면 문서의 특정 문자나 문자열을 검색하여 그 부분만 출력할 수 있다

<test 파일>

![image](https://user-images.githubusercontent.com/94778069/142757268-d8c13ce7-de5c-45f1-acb0-1731a4463619.png)


예를 들어 다음과 같이 하면 test 파일에서 line을 포함한 라인만 출력된다. `awk '/line/' test`

![image](https://user-images.githubusercontent.com/94778069/142758383-fe076ea6-54b4-461b-a847-36fec0ec3da9.png)



위 출력 결과에서 공백 을 구분자로 하여 분리한 필드 중 첫 번째 필드만 출력 `awk -F' ' /line/'{print $1}' test`

![image](https://user-images.githubusercontent.com/94778069/142758610-229515b3-d98b-4dda-8ee2-72209e64636b.png)

등 다양한  awk 사용 방법이있다

