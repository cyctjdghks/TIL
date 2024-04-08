## Querydsl Invalid path 에러
- `Invalid Path`라는 것은 두 엔티티간의 관계가 무엇인지 모르겠다는 것을 의미

이를 해결하기 위해서는 Querydsl에게 두 엔티티간의 관계를 알려주어야 한다고 한다. 즉, join 또는 fetch join할 경우 조인 대상만 명시하지말고 별칭으로 사용할 Q타입까지 명시해주면 해당 문제를 해결할 수 있다.


#### 0. 참고 사이트
https://velog.io/@yshjft/%EC%97%90%EB%B8%8C%EB%A6%AC%ED%83%80%EC%9E%84-%ED%81%B4%EB%A1%A0-%EC%BD%94%EB%94%A9-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%A0%95%EB%A6%AC-JPA

https://okky.kr/questions/1190381