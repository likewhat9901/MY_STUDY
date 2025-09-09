### git repository subtree로 추가하기
```bash
git subtree add --prefix=레포지토리명 레포지토리주소 브랜치명  
ex) git subtree add --prefix=JavaConsoleBankPC https://github.com/likewhat9901/JavaConsoleBankPC main
그리고 git push
or git push origin HEAD:main --force
```
- 추가할 repo가 여러개일 경우 반복한 후 push
출처: https://backendcode.tistory.com/271

---
