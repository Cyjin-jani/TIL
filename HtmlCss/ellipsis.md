## 말줄임표 ellipsis 활용하기
<br>

말줄임표의 효과는 아래와 같이 text-overflow라는 속성을 활용하여 구현이 가능하다. 
<br>

```html
<div class="news">
    <li>동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리 나라만세 무궁화 삼천리 화려강산 대한사람 대한으로 길이 보전하세</li>
    <li>한상혁·이정옥 후보자 청문회…가짜뉴스·갭투자 쟁점</li>
    <li>Hong Kong Arrests Activists Ahead of Sensitive Anniversary</li>
</div>
```

```css
<style>
.news {width:100%}
.news li {
  overflow: hidden; 
  text-overflow: ellipsis;
  white-space: nowrap; 
  width: 90%;
  display: block;
}
</style>
```

white-space:nowrap : 공백문자가 있는 경우 줄바꿈하지 않고 한줄로 나오게 처리함

그러나, 위와 같은 경우는 1줄만 가능하고 만약 특정 라인수 까지는 보여지게 하고 그 초과 분량을 말줄임표를 활용하고 싶은 경우가 있다.
그럴 때 사용하는 것이, -webkit의 활용이다.

아래 속성들을 추가해주면, 특정 라인에서 말줄임 구현이 가능하다.

```css
  display: -webkit-box;
  overflow: hidden;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical; 
```
다만, webkit의 경우는 IE에서 지원이 안되기 때문에, IE를 대응해야 하는 경우라면 사용이 어렵다...