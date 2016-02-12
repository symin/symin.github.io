---
layout: post
title: linux 사용법!
---

<br/>
---
layout: post
title: linux 사용법!
---

<br/>

1. 먼저 우리 서버로 로그인한다. <br/>
``rlogin -l 계정 호스트네임``<br/><br/>

2. deploy 폴더로 이동해서 git에서 소스를 clone 받고 비밀번호를 입력한다.<br/>
``cd deploy``<br/>
``git clone https://symin@github.com/nhnent/basecamp-3rd.6.sun.newspedia.git``<br/><br/>

3. 여기서 혹시 기존의 우리소스가 남아있다면 ``rm -rf`` 명령어를 이용해 기존 폴더를 지운다.<br/><br/>

4. ``cd`` 명령어를 입력하여 홈으로 이동한다. <br/><br/>

5. cd 명령어를 통해  톰캣의 webapps 폴더로 이동! <br/>
  ``cd apps/tomcat/webapps/``<br/><br/>

6.  기존의 newspedia 폴더를 삭제한다. <br/>
    ``rm -rf newspedia``<br/><br/>

7. 빌드하고자 하는 폴더로 이동한다. <br/>
``cd``<br/>
``cd deploy/basecamp-3rd.6.sun.nespedia/``<br/><br/>

8. 빌드시작! -> "BUILD SUCCESS" 라는 명령어를 꼭 확인하기!! <br/>
 ``mvn package`` <br/><br/>
<br/>
---

추가적으로 webapps 폴더로 이동해서 newspedia 폴더를 현재폴더로 옮기는 작업을 수행. 

```
cd

cd apps/tomcat/webapps/

mv /home1/irteam/deploy/basecamp-3rd.6.sun.newspedia/target/newspedia .
```

재시작!

```
cd

scripts/webapps.sh restart
```
---

플러스! TIP!

다음과 같은 명령어를 통해 로그의 내용을 확인할 수 있다!
```
cd apps/tomcat/logs/

tail -f catalina.out


```
ctrl+c -> 탈출! <br/>

```
vim catalina.out 
``` 
q! -> 탈출! <br/> 
만약 ERROR 를 검색하고 싶다면 <br/> 
``/ERROR`` 를 입력한다. <br/>
다음 문자열을 검색하기 위해선 n을 누르고 이전 문자열을 검색하고 싶다면 shift + n을 누른다.<br/> 


## vim을 이용해 디버깅하기 

- vim log/debug.log 명령어를 이용해 프로그램에서 출력한 로그를 상세히 확인한다.
- G 명령어를 이용하면 가장 아래(최신) 생성된 로그의 내용을 확인할 수 있다.
- y 명령어를 이용하면 vim 에서 한 줄 복사가 가능하다. 
- /'문자' 를 입력하면 검색할 수 있다. (?'문자'를 입력하면 뒤에서 부터(최신부터) 검색이 가능하다.)
- 다음 문자로 이동할때는 n을 누르고, 이전 문자로 이동할 때는 shitf + n 을 누른다. 
- 로그파일의 저장된 내용을 확인하고 만약 ERROR 가 발생했을 경우 해당 에러 메세지와 라인을 비교하여 오류를 수정한 후 다시 jar 파일을 생성해 서버로 올려 실행파일을 동작시킨다. 
- ERROR의 내용을 확인해 오류를 확인한다. 



