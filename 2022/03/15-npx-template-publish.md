## npx create-react-app처럼 돌아가는 패키지 배포하기

https://velog.io/@jjunyjjuny/React-TS-boilerplate-%EC%A0%9C%EC%9E%91%EA%B8%B0-%EB%B0%B0%ED%8F%AC-%EB%B0%8F-npx#-boilerplate-%EC%84%A4%EC%B9%98-%ED%8C%A8%ED%82%A4%EC%A7%80

여기에 정말 설명이 잘 되어 있다.  
bin 속성을 활용하여 자동으로 클론해주고 설치하는 스크립트를 불러오면 된다.  

나는 모르고 boilerplate에 bin script까지 넣어서 클론받고 bin dir을 지우도록 수정하였다.

```
npx react-vite-ts-boilerplate my-app
```

이 명령어 만드는거 넘 멋있어 보였는데 만들어 보다니 넘 기분좋다!!
오늘 목적은 vite build 빈파일로 들어가는거 수정하려고 한거였는데 그것도 고치고 이것도 하고 아주 뿌듯하다  
npm publish 하는건 3개 정도 올려 보니 확실히 덜 어색해진 것 같다
