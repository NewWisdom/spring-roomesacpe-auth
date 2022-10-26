# spring roomescape auth
## 1단계 - 로그인
### 기능 요구사항
- [x] 로그인 기능
  - [x] 토큰을 발급 
- [x] 내 정보 조회하기 
  - [x] 토큰을 이용하여 본인 정보 응답하기 
   
### API 설계
#### 토큰 발급
```http request
POST /login/token HTTP/1.1
accept: */*
content-type: application/json; charset=UTF-8

{
    "username": "username",
    "password": "password"
}

```
```http request
HTTP/1.1 200 
Content-Type: application/json

{
    "accessToken": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjYzMjk4NTEwLCJleHAiOjE2NjMzMDIxMTAsInJvbGUiOiJBRE1JTiJ9.7pxE1cjS51snIrfk21m2Nw0v08HCjgkRD2WSxTK318M"
}

```

##### 내 정보 조회
```http request
GET /members/me HTTP/1.1
authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjYzMjk4NTkwLCJleHAiOjE2NjMzMDIxOTAsInJvbGUiOiJBRE1JTiJ9.-OO1QxEpcKhmC34HpmuBhlnwhKdZ39U8q91QkTdH9i0
```
```http request
HTTP/1.1 200 
Content-Type: application/json

{
    "id": 1,
    "username": "username",
    "password": "password",
    "name": "name",
    "phone": "010-1234-5678",
    "role": "ADMIN"
}
```

## 2단계 - 로그인 리팩텅
### 기능 요구사항
- [x] 예약하기, 예약 취소 개선
  - [x] 아래의 API 설계에 맞춰 API 스펙을 변경한다. 
  - [x] 비로그인 사용자는 예약이 불가능하다. 
  - [x] 자신의 예약이 아닌 경우 예약 취소가 불가능하다.
### 프로그래밍 요구사항
- [x] HandlerMethodArgumentResolver를 활용한다.
### API 설계
##### 예약 생성 
```http request
POST /reservations HTTP/1.1
authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjYzMjk4NTkwLCJleHAiOjE2NjMzMDIxOTAsInJvbGUiOiJBRE1JTiJ9.-OO1QxEpcKhmC34HpmuBhlnwhKdZ39U8q91QkTdH9i0
content-type: application/json; charset=UTF-8
host: localhost:8080

{
    "scheduleId": 1
}
```
```http request
HTTP/1.1 201 Created
Location: /reservations/1
```
#### 예약 삭제 
```http request
DELETE /reservations/1 HTTP/1.1
authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaWF0IjoxNjYzMjk5MDcwLCJleHAiOjE2NjMzMDI2NzAsInJvbGUiOiJBRE1JTiJ9.zgz7h7lrKLNw4wP9I0W8apQnMUn3WHnmqQ1N2jNqwlQ
```
```http request
HTTP/1.1 204
```

## 3단계 - 관리자 기능 보호
### 기능 요구사항
- [x] 관리자 역할을 추가한다.
  - [x] 일반 멤버와 관리자 멤버를 구분한다.
- [x] 관리자 기능을 보호한다.
  - [x] 관리자 관련 기능 API는 /admin 붙이고 interceptor로 검증한다.
  - [x] 관리자 관련 기능 API는 authorization 헤더를 이용하여 인증과 인가를 진행한다.

### 프로그래밍 요구사항
- [x] 관리자를 등록하도록 하기 보다는 애플리케이션이 동작할 때 관리자는 추가될 수 있도록 한다
  - [x] DataLoader
