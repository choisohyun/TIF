# 실용주의 프로그래머 📚 3장. 기본 도구

## 3줄 요약

- 하나의 IDE만을 고집하지 않고 기본적인 텍스트 도구들은 바로 사용 가능하게 익혀 두어야 한다.
- 규칙이 있는 일반 텍스트를 사용하면 yaml to json이나 camelCase to snake_case 등의 텍스트 처리를 자유롭게 할 수 있다. 규칙 없는 텍스트는 권장하지 않는다.
- GUI에 종속된다면 모든 command 기능을 사용할 수 없다. 처음 며칠은 힘들더라도 명령어를 노트에 적어 가며 익숙해 지는 과정이 필요하다.

## 책에서 기억하고 싶은 내용을 써보세요.

1. 하나의 IDE만을 고집하여 사용하지 않을 것. 기본 도구(vscode, vim?) 은 언제나 곧바로 사용 가능하게 해야 한다.
2. 일반 텍스트(규칙이 있는 텍스트 = 표준)의 힘
    - 텍스트의 형식만 파악하면 데이터를 파싱하여 사용 가능하다.
    - 버전 관리 시스템에서 변경 기록과 체크섬을 가지고 있어 보존이 가능하다.
    - 테스트에서 사용할 데이터를 일반 텍스트로 표현하면 특별한 도구를 만들 필요 없이 추가/수정할 수 있다.
3. Command shell의 힘을 사용하라
    - GUI의 장점: WYSIWYG(보는 것이 얻는 것)
    - GUI의 단점: WYSIWYG
    - 트랙패드, 마우스 없이 코딩하기(명령어 메모하며 외우기)
4. 디버깅 전략
    - ‘명령 하나'로 버그 재현한다면 버그 고치기가 훨씬 쉬울 것이다. 테스트를 작성해야 한다.
    - 누군가(사람, 고무 오리, 인형, 화분)에게 문제를 설명하는 것만으로도 문제에 대한 새로운 통찰을 얻게 될 수 있다.
5. 텍스트 처리 연습문제
    - yaml to json: javascript에서는 yaml 파싱 모듈을 사용하여 변환한다.
    - camelCase to snake_case
    
    ```jsx
    const camelToSnakeCase = str => str.replace(/[A-Z]/g, letter => `_${letter.toLowerCase()}`);
    camelToSnakeCase('camelCase')
    // output: 'camel_case'
    ```
    

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

- 내일부터는 VSCode를 써 봐야겠다... WebStorm에만 너무 익숙해 버려서 전환점이 필요할 것 같다.
- 터미널 설정 회사 맥북도 설정 바꿔볼 것..! 회사걸 더 많이 써서 더 이쁜 설정이 필요하다.
- `gcb feature/#` , `gco alpha & git pull origin alpha & gm --no-ff feature/#`, ⇒ alias 추가하기
- 에디터 창 쪼개기 명령어 ⇒ shift + command + A + split right
- 찾아보니 webstorm도 내가 몰라서 못쓰던 명령어가 많았다. 내일은 트랙패드 최대한 안쓰고 IDE만 가지고 코딩해 봐야겠다.
