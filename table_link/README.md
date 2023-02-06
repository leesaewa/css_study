# tr 전체 영역에 링크 넣기

- 보통 게시판 리스트는 `table`로 만드는데, 제목 부분에만 링크를 붙이는 경우가 많다.
- 그런데 우리 회사에서는 `tr` 전체에 링크를 붙이길 원했다.
  이렇게 하면 문제가 발생하는데...

## 문제점

- `tr`에는 `a` 태그가 들어가지 않는다. 웹 표준에 `tr` 안팎으로 `a` 태그를 쓸 수가 없다. 오로지 `td` 안에만 `a` 태그를 쓸 수가 있다.

## 해결 방안

1. `tr`에 `onClink`으로 링크를 넣자.

- 문제점
  1. 웹 접근성이 떨어짐.
  - `tab` 버튼이 안 먹히므로, 웹 접근성에 맞지 않다.
  2. `input` 체크박스가 들어가 있는데 선택이 되지 않고 링크로 인식한다. -> 이 경우 해결책이 있는데, 아래에서 상술.
  3. `a` 태그 성애자 검색 로봇에게 외면당함.
     <img src="https://user-images.githubusercontent.com/97646713/212473554-a34ed1c0-b601-46d6-85f9-9de11cbd1592.png">

2. `td`마다 `a` 태그를 넣자

- 문제점
  1. 유지보수에 좋지 않음. ~~노가다~~

3. `jquery`로 핸들링한다

- 검색해봤을 때 나온 방법이다.
- 강제로 `tr`에 `a`태그를 넣는 방법을 설명하고 있었다. 이렇게 하면 검색 로봇에도 걸린다고... 이것도 문제는 있었지만 시도해볼만 하다고 생각했다.
- <a href="https://xetown.com/tips/1152143">참고 URL</a>

---

- 이런 방법들이 있었는데, 이걸 작성한 시점에서 내가 해결한 방법이 있었다.

## `css` 가상 클래스를 이용해보는 것!

- 하나의 `td`에만 `a` 태그를 넣고 `a`태그의 가상 클래스 `::before`에 영역을 지정해줬더니 `tab`버튼에도 잡히고, 전체 클릭도 가능하며, `a`태그라서 검색 로봇에도 걸린다.

### html 코드

```
<table>
      <tr>
        <td>text</td>
        <td>text</td>
        <td>text</td>
        <td><a href="">Link</a></td>
        <td>text</td>
      </tr>
    </table>
```

### CSS 코드

```
table {
  width: 100%;
  border-collapse: collapse;
}
tr {
  position: relative;
}
tr:hover {
  background-color: #ddd;
}
td {
  border-bottom: 1px solid #ddd;
  padding: 5px;
}
a {
  display: block;
  text-decoration: none;
}
a::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```

---

## 느낀점

- CSS의 놀라움은 끝이 없다. 이래서 CSS를 버릴 수가 없다.
- javascript도 좋지만, 웬만한 건 css로 해결해보자.
- 아무리 유저에게 노출되지 않는 페이지라고 해도 웹 표준과 웹 접근성은 중요한 것 같다.
