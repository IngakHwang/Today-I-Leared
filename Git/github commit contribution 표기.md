# github - commit 후 contribution 표기 안됨



회사 이메일 계정과 깃허브 이메일 계정이 달라 contribution 표기가 안됨



contribution 그래프 채워지는 조건 3가지

1. 커밋 시 사용한 이메일 주소가 github계정의 이메일 주소와 같은지
2. fork 한 commit은 적용되지 않고 독립적인 repository에서 이루어진 commit 이여야 한다.
3. commit은 다음으로 만들어져야 한다.
   - repository의 default branch (보통 master)
   - github page branch



## user 이메일 변경법

`git config user.email` -> 등록된 이메일 확인



`git config --global user.email 바꿀이메일주소` -> 이메일 변경





---

참고 사이트

velog - https://velog.io/@think2wice/Github-%EB%B6%84%EB%AA%85-commit%EC%9D%84-%ED%96%88%EB%8A%94%EB%8D%B0-%EC%99%9C-contribution-%EA%B7%B8%EB%9E%98%ED%94%84%EB%8A%94-%EC%95%88%EC%B1%84%EC%9B%8C%EC%A7%80%EC%A7%80

tistory - https://develaniper-devpage.tistory.com/76