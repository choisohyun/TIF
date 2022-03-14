## SvelteKit에는 왜 Vite가 선택되었을까? `Vite VS Snowpack`

- svelte 인프런 강의를 보다가 snowpack으로 되어 있길래 강사님에게 빌드툴 선택에 대한 질문을 올렸더니 아래 유투브를 공유해 주셨다.
- [유투브 링크](https://www.youtube.com/watch?v=tUXqcrHrGJk&ab_channel=BraydenGirard)
- 내가 이해한 내용으로 SvelteKit에서 Vite를 선택한 이유를 간단히 정리해 보려고 한다. (유투브가 21년도 3월 영상이라 현재와 다를 수 있다.)

1. Vite가 버전이 1에서 2로 올라가면서 지원하는 기능들이 좋아졌다.
2. Vite 사용 시 작성하는 코드가 적어졌다. 
  - snowpack을 사용할 경우에는 css 코드 스플리팅과 소스맵으로 스택 추적하는 기능에 대한 코드를 많이 써야 했다.

  ```js
  // svelte.config.js
  // 기본 코드가 매우 적다. CRA나 Vue-cli에서 숨어 있는 webpack 코드를 생각하면 정말 적다.. 
  const config = {
        kit: {
                adapter: adapter(),

                // Override http methods in the Todo forms
                methodOverride: {
                        allowed: ['PATCH', 'DELETE']
                }
        }
  };
  ```
3. Vite는 rollup을 베이스로 두고 있어 rollup plugin을 자유롭게 사용할 수 있다.
4. SvelteKit maintainer가 프로젝트에서 종속성을 제거할 수 있었다.
  - 9개로 존재하던 종속성을 3개로 줄일 수 있었다.
  
  ```js
  // svelte kit 기본 모드로 생성할 경우 dependencies
  "dependencies": {
    "@fontsource/fira-mono": "^4.5.0",
    "@lukeed/uuid": "^2.0.0",
    "cookie": "^0.4.1"
  }
  ```

### 결론

- vite나 snowpack 모두 빠르고 작게 가져가고자 하는 방향성은 일치하는 것 같다. 하지만 지원하는 유연성과 퀄리티가 Vite가 더 좋다고 생각된다. Vite가 rollup based로 했던 게 탁월한 선택인 것 같다.
- 하지만 레거시 프로젝트에서 vite로 바꾸는 것에 대해서는 더 많은 고민이 필요하다. 공식적으로 지원되는 모던한 버전의 디펜던시를 가지고 있는 프로젝트라면 vite로 변경할 것 같다. [참고](https://velog.io/@sehyunny/is-it-time-to-say-goodbye-to-webpack#%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%9B%B9%ED%8C%A9%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%A4%91%EB%8B%A8%ED%95%98%EA%B3%A0-vite%EB%A1%9C-%EB%B0%94%EA%BF%94%EC%95%BC%ED%95%A0%EA%B9%8C%EC%9A%94)
