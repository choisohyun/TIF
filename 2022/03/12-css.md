
### css로 0 문자 구분자 표시하기

- `font-variant-numeric: slashed-zero`
- mdn 예제를 보면 구분자가 스타일에 따라 바뀌는 것을 볼 수 있다. https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric
- 하지만 언제나 바뀌지는 않는다. font-family가 특정한 것이어야만 작동되는 것 같다. mdn에서도 `sans-serif` 폰트를 해제하면 스타일이 적용되지 않았다. https://www.quackit.com/css/css3/properties/css_font-variant-numeric.cfm

![IMG_3494](https://user-images.githubusercontent.com/30427711/158021110-5b1fcb1a-bbdd-498e-9012-38e3c3256d3f.JPG)

### img `decoding="async"`

- image를 모두 로드시킨 후 load 처리되는 것이 아니라 이미지는 비동기로 디코딩시키게 하는 속성이다.

![IMG_3499](https://user-images.githubusercontent.com/30427711/158021891-476f644f-687e-4af8-990e-bf9b0584b849.JPG)

- 예제를 보면 이벤트리스너로 load 시점을 찍었을 때 load가 먼저 찍히는 것을 알 수 있다. skeleton loading이랑 비슷한데 이거는 디코딩할 시간까지 랜더링에서 제거할 수 있어서 속도를 높이는 데에 도움이 더 될 것 같다. 이미지가 많은 페이지에 써봐야겠다
- https://github.com/prof3ssorSt3v3/image-decode-attr/blob/main/main.js
<img width="375" alt="Screen Shot 2022-03-12 at 11 41 32 PM" src="https://user-images.githubusercontent.com/30427711/158022474-d43b373e-2800-41d8-ab1c-11724c1793ee.png">

출처: https://www.youtube.com/watch?v=a_vsIfgsSCQ&ab_channel=SteveGriffith-Prof3ssorSt3v3

- caniuse를 보면 대부분의브라우저에 지원되고 있다. 크롬은 99부터로 나오는데 99가 현재 최신 버전이어서 확인은 좀더 필요할 것 같다. https://caniuse.com/mdn-html_elements_img_decoding
