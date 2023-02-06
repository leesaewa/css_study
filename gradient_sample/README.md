# border_gradation

input border gradation with SVG

## Goal

- `input`이 `hover`와 `focus`되었을 때 `border gradient`가 나오도록 할 것.
- 반드시 `border-radius`가 포함되어야 함.
- `input`의 `background-color`는 무조건 `opacity`값이 들어가야 함.

## 시도

### <a href="https://developer.mozilla.org/ko/docs/Web/CSS/border-image">border-image</a>

```
.border-image .input-box input:hover {
  border: 1px solid transparent;
  border-image: linear-gradient(to right, #000046 0%, #1cb5e0 100%);
  border-image-slice: 1;
}
```

#### 실행 결과

<img src="https://user-images.githubusercontent.com/97646713/212069526-f04ab59c-54ec-450b-9f21-62448ebdb30b.png">

#### 문제점

- `border-image` `border-image-slice`속성을 이용해서 그라데이션을 넣을 수가 있지만 `border-radius`를 넣을 수가 없음...
- `border-image` 속성과 `border-radius` 속성이 호환되지 않기 때문이라고 함.

---

### <a href="https://developer.mozilla.org/ko/docs/Web/CSS/background-origin">background-origin</a>, <a href="https://developer.mozilla.org/ko/docs/Web/CSS/background-clip">background-clip</a>

```
.bg-image .input-box input:hover {
  border: 1px solid transparent;
  background-image: linear-gradient(#fff, #fff),
    linear-gradient(to right, #000046 0%, #1cb5e0 100%);
  background-origin: border-box;
  background-clip: content-box, border-box;
}
```

- `background`를 이용해서 `border-radius`를 넣는 방법.
- `border`의 색을 투명하게 하고, `background`에 `background: linear-gradient(컨텐츠 색상)` 과 `linear-gradient(보더 색상);`를 설정해주면 됨.
- `border-radius`는 적용되지만...

#### 실행 결과

  <img src="https://user-images.githubusercontent.com/97646713/212054177-54a64a28-e134-416d-a795-4c9e606cedae.png">

#### 문제점

- `padding` 값을 넣을 수가 없고, `background`에 `opacity`를 줄 수 없음.

## 해결법

- 그래서 생각해본 게 `SVG`.
- `SVG`는 내가 원하는대로 그릴 수 있기 때문에 `SVG`로 `border-radius`가 포함된 선을 그리기
- `opacity`로 가리고 `hover`했을 때에만 `opacity` 값을 `1`로 변경

```
.gradient .input-box svg {
  cursor: text;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
  transition: all 0.5s ease-in-out;
}
.gradient .input-box svg rect {
  width: calc(100% - 2px);
  height: calc(100% - 2px);
}

.gradient .input-box label:hover svg {
  opacity: 1;
}
.gradient .input-box input:focus + label svg {
  opacity: 1;
}
.gradient .input-box input:focus {
  background-color: rgba(255, 255, 255, 0);
  outline: none;
  border: 0;
}
.gradient .input-box input:focus::placeholder {
  color: #fff;
}
```

### 실행 결과

<img src="https://user-images.githubusercontent.com/97646713/212070118-1d779d5a-e944-40b1-a592-2364bdbd6a25.png">

### 문제점

1. 코드가 길어짐...
2. `input`이 들어가는 곳마다 `SVG`를 넣어줘야만 하는 번거로움...
