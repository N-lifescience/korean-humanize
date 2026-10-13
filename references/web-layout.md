# 웹 레이아웃 기준

다음은 적용 패턴이다. 기존 디자인 토큰과 컴포넌트를 우선하고 관련 요소에만 스타일을 적용한다. 한국어 전용 화면은 `lang="ko"`를 사용하고, 여러 언어가 섞인 화면은 한국어 영역에 언어를 지정한다.

## 기본 줄바꿈

```css
.ko-ui {
  word-break: keep-all;
  overflow-wrap: anywhere;
  line-height: 1.6;
}

.ko-ui :where(h1, h2, h3, .ui-title) {
  line-height: 1.3;
  text-wrap: balance;
}

.ko-ui :where(p, .ui-description, .ui-error) {
  text-wrap: pretty;
}

.ko-ui :where(.layout-item, .text-content) {
  min-inline-size: 0;
  max-inline-size: 100%;
}

.ko-ui :where(button, .button) {
  font: inherit;
  white-space: normal;
  line-height: 1.4;
  min-block-size: 2.75rem;
  padding-block: 0.7rem;
  padding-inline: 1rem;
}
```

`overflow-wrap: anywhere`는 어절 전체가 한 줄에 들어갈 수 없을 때의 비상 처리다. 일반 한국어 어절은 먼저 공백에서 감싸야 한다. 기존 `word-break: break-all`이나 `white-space: nowrap`이 우선 적용되는지도 확인한다.

`text-wrap: balance`는 짧은 제목에, `pretty`는 설명에 선택적으로 사용한다. 브라우저와 줄 수에 따라 효과가 달라지므로 지원 여부만 보고 결과를 판정하지 않는다. 무거운 화면의 모든 셀에 불필요하게 적용하지 않는다.

## 공간이 부족할 때

- flex 자식은 `min-inline-size: 0`, grid의 유연한 칸은 `minmax(0, 1fr)` 등으로 줄어들 수 있게 한다. 부모의 `min-width`, 고정 폭, `overflow: hidden`도 함께 확인한다.
- 아이콘은 필요하면 `flex-shrink: 0`으로 보존하고 글 영역이 감싸지게 한다. 아이콘 자체를 글자의 기준선에서 떨어뜨리지 않는다.
- 버튼 묶음은 감싸기, 세로 배치, 열 수 조정으로 해결한다. 버튼의 고정 높이를 풀어 두 줄도 표시할 수 있게 한다.
- 본문과 입력의 글자 크기는 기존 접근성 기준을 지키며 보통 1rem 수준에서 시작한다. 화면에 넣으려고 핵심 문구를 계속 줄이지 않는다. 보조 설명도 대비와 읽기 크기를 확보한다.
- 행간과 여백은 고정 높이에 가두지 않는다. 긴 오류와 설명이 추가돼도 아래 요소를 밀어낼 수 있게 한다.
- 문자 200% 확대에서 좌우 여백까지 커져 글 영역을 잠식하거나, 화면 폭 기준의 열 수가 그대로 유지되는지 확인한다. 필요하면 좌우 여백을 유연하게 줄이고 내용이 들어가는 최소 폭을 기준으로 열을 줄인다. 글자 크기를 원래대로 돌려 해결하지 않는다.
- 화면 전체의 가로 넘침을 `overflow-x: hidden`으로 가리지 않는다. 의도적으로 스크롤하는 표나 코드 영역은 해당 영역에만 스크롤을 제공하고 주변 안내는 정상적으로 감싸지게 한다.

## 공백과 의미 단위

의미상의 문단은 HTML 문단이나 블록으로 표현한다. JSX 소스에서 줄만 나눈 것이 실제 줄바꿈인지, 문자열 결합 과정에서 어절 사이 공백이 사라졌는지 확인한다. 반대로 화면 폭 때문에 문자열에 임의의 공백이나 개행을 넣지 않는다.

주소, 전화번호, 수치와 단위처럼 함께 읽어야 하는 짧은 표현도 무조건 `nowrap`으로 묶지 않는다. 가장 작은 폭과 확대 상태에서 묶음이 실제로 들어가는지 먼저 확인한다. 공식 표기와 사용자의 명시적 요구가 있으면 그것을 따른다.

확인 항목은 음절 분리, 어절 경계, 문장의 의미, 문단 간 연결 순서다. 마지막 줄의 글자 수를 줄이는 것이 의미 보존보다 우선하지 않는다.

## 참고 문서

- [MDN word-break](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/word-break)
- [MDN overflow-wrap](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/overflow-wrap)
- [MDN text-wrap-style](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/text-wrap-style)
