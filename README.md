# openSource project #1
*** 
**To Do List**

* getopt 명령어 조사
* getopts 명령어로의 발전 조사
* sed 명령어 조사
* awk 명령어 조사

> getopt 명령어


getopt 용도
  * 명령행 플래그와 매개변수를 구문 분석

getopt를 사용하는 이유

1) 다양한 입력 값이 존재할 경우 사용자와 개발자의 편의를 보장하기 위함
2) 스크립트를 보다 체계적으로 관리할 수 있음

설명 

getopt 명령은 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석합니다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 합니다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없습니다.

getopt 명령은 **모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이** 발생하는 경우 처리를 완료합니다. 그러면 getopt 명령은 처리된 플래그, -(더블 하이픈) 및 남아 있는 토큰을 출력합니다.

다음 과 같이 getopt 명령을 쉘 스크립트에서 사용하여 옵션을 구문 분석할 수 있습니다.

\\\
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
\\\

getopts 명령의 경우 옵션들 중간에 파일명이 온다거나 하면 이후의 옵션은 옵션으로 인식이 되지 않는데 getopt 명령의 경우는 올바르게 구분하여 정렬해 줍니다.

\\\
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
\\\
* getopts

\\\
$ ./test.sh -a 123 hello.c -bc
-a was triggered!, OPTARG: 123
------------------
hello.c -bc
\\\
* getopt
\\\
#!/bin/bash

options=$(getopt -o a:bc -- "$@")
echo $options
.................................

$ ./test.sh -a 123 hello.c -bc
-a '123' -b -c -- 'hello.c'
\\\
