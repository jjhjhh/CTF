## **Introspection | 100 | web**

Welcome to the Secret Agents Portal. Find the flag hidden in the secrets of the Universe!!!

http://34.16.207.52:5134/ index.html코드를 보면 script.js를 불러오는 것을 확인할 수 있다 

```jsx
<body cz-shortcut-listen="true">
    ...
    <script src="script.js"></script>
</body>
```

<br>

http://34.16.207.52:5134/script.js 소스코드에서 flag 확인 

```jsx
function checkFlag() {
    const flagInput = document.getElementById('flagInput').value;
    const result = document.getElementById('result');
    const flag = "OSCTF{Cr4zY_In5P3c71On}";
    ...
}
```

<br>

## **Style Query Listing...? | 100 | web**

pfft.. 들어보세요. 이 로그인 포털에 액세스했지만 로그인할 수 없습니다. 관리자가 확실히 대중에게 뭔가를 숨기고 있는 것 같지만... 무엇인지 이해가 안 됩니다. 여기 링크를 가져가서 조용히 하세요. 누구와도 공유하지 마세요.

제목만 보고 CSS injection인 줄 알았는데 http://34.16.207.52:3635/admin 에 접근했더니 플래그가 떴다. `OSCTF{D1r3ct0RY_BrU7t1nG_4nD_SQL}`

<br>

## **Weird Video | 100 | misc**

여기요! 내 컴퓨터에 이 mp4 파일이 놓여 있는 것을 발견했는데 이상하게도 열 수 없나요? 여기 좀 보세요.

[content.mp4](https://ctf.os.ftp.sh/files/5d197a440fdecabbe243c330320c785d/content.mp4?token=eyJ1c2VyX2lkIjo2NjQsInRlYW1faWQiOjQxMSwiZmlsZV9pZCI6MjR9.ZpIzlw.nmhUlbSvUdAvEWEnKtyPaJtqxzs)

hxd로 열면 flag가 보인다. `OSCTF{T3xt_3DiT0r_FTW!}`

<br>

## **Cyber Quiz | 100 | misc**

My teacher assigned me this quiz and told me that I have to answer each question correctly otherwise I won't be able to pass the test. Can you help me? Pwease

nc에 접속해서 나오는 문제를 풀면 플래그가 하나씩 공개된다.

Final flag: OSCTF{L33__Kn0w__Dg3}

나머지 빈칸은 추측했다. **`OSCTF{L33t_Kn0wl3Dg3}`**

<br>

## **Find the Flag | 100 | misc**

There is a Flag hidden somewhere in the realms of code of this ctf server... but the question is, Where!?

https://ctf.os.ftp.sh/ 페이지 주석에 숨어있다. 

<!--Flag: `OSCTF{D1d_y0u_F1nd_m3?!}` -→
