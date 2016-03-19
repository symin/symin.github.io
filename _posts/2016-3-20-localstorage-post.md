---
layout: post
title: LocalStorage에 대해 알아보자!
---


## 문제 

뉴스 웹사이트 `Newpedia`는 로그인 하면 항상 main 화면으로 redirect 되도록 구현하였습니다.  그러다 보니 기사 상세보기 화면에서 댓글을 쓰기 위해 로그인을 하면 다시 main 페이지로 돌아간다는 이슈가 있었습니다.

버그도 아니고 중요한 핵심 기능도 아니었기에 조용히 넘어가려 하였지만, 사용자의 편리성과 만족성을 위해 개선사항으로 이슈를 해결하였습니다 :)

## 생각

### 로그인 하기 전의 User의 마지막 url을 어디에, 어떻게 저장할까? 

이번 교육을 통해 처음 Redis를 접한 이후로 `저장!` 이라는 단어를 들으면 `Redis`를 떠올랐습니다. 그래서 url도 마찬가지로 redis에 저장하고자 생각하였는데 굳이 서버에 까지 저장할 필요가 없다고 판단하여 클라이언트 단에서 해결하고자 폭풍 구글링을 시작하였습니다.

## 해결

### LocalStorage

LocalStrorage 를 사용하면 클라이언트 디바이스에 직접 데이터를 저장하고 가져올 수 있습니다. 사용법은 정말 정말 쉽게 사용할 수 있어요.

데이터는 key, value 쌍으로 구성되고 key, value 모두 String으로 저장됩니다.

```
// Save data to the current local store
localStorage.setItem("username", "John");

// Access some stored data
alert( "username = " + localStorage.getItem("username"));
```

이렇게 setItem(), getItem() 으로 값을 저장하거나 가져올 수 있습니다. 

---
### 적용
```
var url = location.href;
localStorage.setItem("url", url);
```

```
<script> 
  var url = localStorage.getItem("url");
  location.href = url;
</script>
```

-> 사용자가 로그인 버튼을 누르면 localStrorage에 현재 url을 저장시키고  redirect.jsp 생성하여 localStorage에 저장된 url로 redirect 시키는 방식으로 적용하였습니다!

추가적으로 유용한 매서드와 속성은 다음과 같습니다. 

```
    localStorage.removeItem(키); // 해당 키 삭제
    localStorage.clear(); // 모두 지운다.

    localStorage.length; // 저장된 키의 개수
    localStorage.key(값); // 값으로 키를 찾는다.
 ```
 

참고 :  `https://developer.mozilla.org/en-US/docs/Web/API/Storage/LocalStorage`
