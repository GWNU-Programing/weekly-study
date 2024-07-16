Git, GitHub는 팀 협업을 하면서 소스코드를 공유하고 버전을 관리하기 위해 필수적인 툴이라고 생각한다.

그동안 기본적이고 필수적인 요소만 알고 있었는데 한번 공부해 볼 필요가 있다고 생각하여 정리하고자 한다.

# Git 이란?

분산 버전 관리 시스템으로 소프트웨어 개발에서 소스 코드의 변경을 추적하는데 사용된다. <br>
많은 개발자들이 팀프로젝트를 할때 관리하고 협업하는데 많이 사용되고 있다. <br>
주로 로컬 컴퓨터에서 cmd를 통해 여러가지 기능을 사용한다. 

## 특징

- 코드 버전 관리
- 브랜치 생성 및 병합
- 과거 버전으로 되돌리기
- 분산형 저장소 구조

# Git 동작 개념

Git이 어떻게 동작하는지 알기 위해서 작업 영역과 파일 상태에 대해 이해해야한다. 

## Git 작업 영역

작업한 파일들을 기록하고 저장하기 위해서 이러한 영역으로 나누어 놓았다. 

![](https://velog.velcdn.com/images/hwstar-1204/post/76c30d62-c63a-4970-b0c5-fef576b5440e/image.png)
>
- **Working Directory **
    : 처음에 작업중인 파일이나 코드가 여기에 위치하여 추가, 수정하면 git이 변경사항을 자동으로 감지한다. 
- **Staging Area**
    : working directory에서 기록하고싶은 파일을 add 명령어로 스테이징 했을 경우 파일의 위치     
- **Repository** (Local)
    : 스테이징된 파일들을 저장하기 위해 commit 명령어를 수행했을 경우 파일의 위치 
    

## Git 파일 상태

이러한 파일들의 상태를 확인하면 어떤 작업 영역에 위치하는지 알 수 있다. 

![](https://velog.velcdn.com/images/hwstar-1204/post/226ac03e-428f-4ffd-af17-8da13c03c759/image.png)

>
- **Untracked**
    : 기존에 관리하고 있지 않았던 파일이 새롭게 추가된 상태 
- **Unmodified**
    : 기존에 관리하고 있던 파일이지만 수정되지 않은 상태
- **Modified**
    : 기존에 관리하고 있던 파일이고 수정된 상태 
- **Staged**
    : 변경 사항을 기록하기 위해 add 명령어로 스테이징한 상태 
- **Comitted**
    : 변경 사항을 로컬 저장소에 저장히기 위해 commit 명령어로 커밋한 상태 
    

## Staging Area 용도

여기에서 중간 영역인 “Staging Area 없이 바로 Repository로 commit하면 되는거 아닌가?” 하는 의문이 들 수 있다. 

이 영역이 필요한 이유는 여러모로 쓸모 있다. 

1. 일부 파일만 커밋할 경우
    
    여러가지 파일을 수정했는데 한번에 커밋을 하여 기록하기보다 어떤 기준에 따라 커밋을 나눠서 하면 버전을 추적하기도 수월하고 추후 커밋 기록을 보았을 때 어떤 작업을 했는지 잘 알아볼 수 있다. 
    
2. 충돌을 수정할 경우
만약에 여러 사람들이 하나의 repository에서 작업을 할때 코드를 합치는 과정(merge)에서 충돌이 날 수 있는데 이때 충돌 나는 부분만 staging area에서 해결하여 커밋할 수 있다. 
3. 커밋을 수정할 경우
    
    커밋을 했는데 잘못 커밋을 했을 때 해당 커밋을 staging area으로 복구 시켜서 수정하고 다시 커밋 할 수 있다. 
    
# 명령어 사용에 따른 저장소 변화


git status는 저장소의 상태를 확인하는 명령어이다. 이 명령어를 통해 저장소의 상태를 단계별로 살펴보자 

![](https://velog.velcdn.com/images/hwstar-1204/post/09520d1f-f9c1-45d3-a51f-006d180a83bf/image.png)

아무 변경사항이 없는 초기 저장소의 상태이다. ( 현재 작업 위치: ```Working Directory``` )

![](https://velog.velcdn.com/images/hwstar-1204/post/cc36e285-30af-4d67-9046-b5c30b23de50/image.png)

main.py 파일을 수정한 후 저장소의 상태이다. 파일의 수정을 git이 감지하여 파일의 상태가 Unmodified -> Modified 상태로 바뀐 모습을 볼 수 있다. <br>
하지만 아직 commit을 위해 staging area에 추가되지 않았음을 알려준다. <br>
친절하게 스테이징 하는 명령어 add 사용법을 알려주고 
내가 파일에 수정한 내용을 삭제하고 원본 working directory로 되돌리는 명령어 restore 사용법을 알려준다. 

![](https://velog.velcdn.com/images/hwstar-1204/post/dc91a540-023b-42e3-a9e3-c4ce937d5083/image.png)

add 명령어로 main.py의 변경사항을 스테이징 하였다. ( 현재 작업 위치: ```Staging Area``` )<br>
친절하게 스테이징 영역에서 working directory로 옮길때는 restore 명령어에 --staged 옵션으로 하라고 알려준다. 

![](https://velog.velcdn.com/images/hwstar-1204/post/13c641a5-8385-4268-aeb8-5779859bf573/image.png)
commit 명령어로 스테이징 영역에서 커밋하였다. ( 현재 작업 위치: ```Local Repository``` )
-m 옵션을 통해 바로 커밋 메시지를 작성할 수 있다. 

커밋을 하게되면 기록마다 해시값이 지정이된다. 추후 개발을 하다가 해당 커밋한 위치로 돌아오거나 파일을 비교하거나 할 때 필요한 놈이다. <br>
이 해시값은 고유하므로 다른 개발자도 이 해시값만 알면 해당 커밋의 내용을 볼 수는 있다. (대부분은 커밋 메시지로 구분하겠지만)<br>
[master e292aaa] : 현재 master 브랜치에서 커밋한 기록의 해시값은 'e292aaa' 이라고 알려준다. 

## Command Tip

``` bash
git add . 	# working directory에 추가,수정된 모든 파일을 한번에 스테이징 가능
git commit -am "commit msg"		# add와 commit을 한꺼번에 명령 가능  
git restore "filename"		# working directory에서 수정한 내역 삭제 
git restore --staged "filename"		# staging area -> working directory
```

# 느낀점
git으로 로컬 환경에서 여러 작업 공간을 두고 CLI 기반으로 파일을 관리하는 방법을 알 수 있었다. <br>
github desktop으로 간단하게 하던 작업들이 어떻게 작동하는지 자세히 알 수 있었다. 

