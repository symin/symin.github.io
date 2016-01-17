---
layout: post
title: OAuth 인증 이해
---

들어가기 전에.....
* http://www.naver.com/logo.png 파일을 요청하는 HTTP Request?

```
GET /logo.png HTTP/1.1
Host: www.naver.com
```

* 해당 자원이 없을 때 서버 응답은?

```
HTTP/1.1 404 Not Found
```

기술교육 첫날 시험 문제였는데 간단한 질문이였지만 나는 정답을 쓰지 못했다. <br/> 
책을 읽을때 좀 더 꼼꼼히 생각하고 공부하며 읽어야 겠다는 생각을 했다.  <br/><br/>

## OAuth란? 
OAuth에서 'Auth'는 'Authentication'(인증)뿐만 아니라 'Authorization'(허가) 또한 포함하고 있다. <br/>
그렇기 때문에 OAuth 인증을 진행할 때 해당 서비스 제공자는 '제 3자가 어떤 정보나 서비스에 사용자의 권한으로 접근하려 하는데
허용하겠느냐'라는 안내 메시지를 보여 주는 것이다. <br/>
```
User	- Service Provider에 계정을 가지고 있으면서, Consumer를 이용하려는 사용자<br/>
Service Provider	- OAuth를 사용하는 Open API를 제공하는 서비스 <br/>
Consumer	- OAuth 인증을 사용해 Service Provider의 기능을 사용하려는 애플리케이션이나 웹 서비스 <br/>
Request Token	- Consumer가 Service Provider에게 접근 권한을 인증받기 위해 사용하는 값. 인증이 완료된 후에는 Access Token으로 교환한다. <br/>
Access Token	- 인증 후 Consumer가 Service Provider의 자원에 접근하기 위한 키를 포함한 값 <br/>
```

![인증과정](http://d2.naver.com/content/images/2015/06/helloworld-24942-3.png) <br/>
OAuth 1.0a 인증 과정 <br/>

``출처: http://oauth.net/core/diagram.png``

## 네이버 OAuth 인증 

### 인증코드 요청
#### [request]
```
RequestURL: https://nid.naver.com/oauth2.0/authorize?response_type=code&client_id=jyvqXeaVOVmV&redirect_uri=&state=rki4tvChHQaLdUb2

GET /oauth2.0/authorize?response_type=code&client_id=jyvqXeaVOVmV&redirect_uri=&state=rki4tvChHQaLdUb2 HTTP/1.1
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/6.0)
Host: nid.naver.com
Pragma: no-cache
Accept: */*
```
#### [response]
```
GET /oauth/naverOAuthExp.nhn?code=QSIPbVZoUCTQ5vs1&state=rki4tvChHQaLdUb2 HTTP/1.1
Host: nid.naver.com
```

인증 요청문 형식
```
https://nid.naver.com/oauth2.0/authorize?client_id={클라이언트 아이디}&response_type=code&redirect_uri={개발자 센터에 등록한 콜백 URL(URL 인코딩)}&state={상태 토큰}
```
각각의 파라미터
```
client_id: 애플리케이션 등록 후 발급받은 클라이언트 아이디
response_type: 인증 과정에 대한 구분값. code로 값이 고정
redirect_uri: 네이버 로그인 인증의 결과를 전달받을 콜백 URL(URL 인코딩).
애플리케이션을 등록할 때 Callback URL에 설정한 정보
state: 애플리케이션이 생성한 상태 토큰
```

### Acesstoken 요청
#### [request]
```
POST /v1/nid/getUserProfile.xml HTTP/1.1 User-Agent: naver oauth2.0 test form Host: opanapi.naver.com Pragma: no-cache Accept: */* Authorization: Bearer AAAAQnCTEH34tM6DAFaXZi8B09z7wIXbt65ag6N8Kf/XN0i8R7t7Hww3Pfzs1TqCRVbyvAKPXUYeg
```
#### [response]
```
HTTP/1.1 200 OK
Date: Sun, 17 Jan 2016 07:21:24 GMT
Server: Apache
Set-Cookie: nid_nalx=JvVssOgMUvnxwXI1i6b3VITOTkEmo+WmQW+CZCdA8Rk=; expires=Wed, 16-Jan-2019 07:21:24 GMT; path=/; domain=.nid.naver.com; Secure; HttpOnly
Pragma: no-cache
P3P: CP="ALL CURa ADMa DEVa TAIa OUR BUS IND PHY ONL UNI PUR FIN COM NAV INT DEM CNT STA POL HEA PRE LOC OTC"
Cache-control: no-cache
Expires: now
Expires: -1
Cache-Control: no-store, no-cache, must-revalidate
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 331
Connection: close
Content-Type: application/json; charset=euc-kr

{ "access_token":"AAAAQnCTEH34tM6DAFaXZi8B09z7wIXbt65ag6N8Kf/XN0i8R7t7Hww3Pfzs1TqCRVbyvAKPXUYegE3Pj05Q6h3HpETUbDrba6iUeC4vb4toGGGO", "refresh_token":"4MmTSToisPipkVbis7pEemy2RcueWGulyFkREfRgO2dMhSZXw07DxsEMBj1conY7rbbiiyMUDv2d4VeQkrgTU7Ex7BINq3ImKiitPg64kPuLglzkQrWaafddvTt8elipYnSW74", "token_type":"bearer", "expires_in":"3600" }
```

### Test Api 호출

#### [request]
```
POST /v1/nid/getUserProfile.xml HTTP/1.1 User-Agent: naver oauth2.0 test form Host: opanapi.naver.com Pragma: no-cache Accept: */* Authorization: Bearer AAAAQnCTEH34tM6DAFaXZi8B09z7wIXbt65ag6N8Kf/XN0i8R7t7Hww3Pfzs1TqCRVbyvAKPXUYeg
```
#### [reponse]
```
HTTP/1.1 200 OK
Date: Sun, 17 Jan 2016 07:23:26 GMT
Server: Apache
Cache-Control: no-cache, must-revalidate
Content-Length: 571
Connection: close
Content-Type: text/xml;charset=utf-8

<?xml version="1.0" encoding="UTF-8" ?>
<data><result><resultcode>00</resultcode><message>success</message></result>
<response><email><![CDATA[tjsdb573@naver.com]]></email>
<nickname><![CDATA[tjsd****]]></nickname>
<enc_id><![CDATA[a566cf985fdf5803550960ee4d016e3fdcf5020c8c3deed60368bda8de3fc096]]></enc_id>
<profile_image><![CDATA[https://ssl.pstatic.net/static/pwe/address/nodata_33x33.gif]]></profile_image>
<age><![CDATA[20-29]]></age>
<gender>F</gender>
<id><![CDATA[12325605]]></id>
<name><![CDATA[민선유]]></name>
<birthday><![CDATA[08-10]]></birthday></response></data>
```

사용자 프로필 정보 조회 요청 API를 이용해서 <br/>
XML 형식으로 해당 사용자의 정보를 받아오는 과정을 확인할 수 있었다. <br/>

요청과 응답에 대한 결과를 확인해보고, <br/>
email, nickname, age, gender, 이름, 생일 등 직접 나의 정보를 받아오는 과정을 보니 조금 더 명확하게 이해할 수 있었다!<br/><br/>

``참고 사이트 
http://static.nid.naver.com/oauth/naverOAuthExp.nhn, 
https://nid.naver.com/devcenter/docs.nhn?menu=Web
`` 

