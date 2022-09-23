## CSS 애니메이션

- keyframe을 이용해서 애니메이션을 제작하고, 애니메이션을 실행시킬 태그 css 내부에 animation : animation 명 + 추가 속성 을 기입하여 사용
- keyframe : 애니메이션을 작성 0%~100% 까지 해당 css의 애니메이션을 설정
- animation : keyframe의 설정한 애니메이션을 사용할 태그의 css에 설정, 띄어쓰기로 각 설정을 구분
  - animation : keyframe명 name duration delay direction iteration-count play-state time-function fill mode
  - duration : keyframe 0% -> 100% 의 변화의 시간을 얼마나 줄 것인지
  - delay : keyframe이 시작하기 전 대기하는 시간
  - direction : 재생 방향을 지정할 수 있음
    - normal : 0%에서 시작
    - reverse : 100%에서 시작 -> 0%로 도착
    - alternate : iteration-count를 2이상으로 해주면 왔다(1회) 갔다(1회) 반복(1+1 = 2)
  - iteration-count : keyframe 반복횟수, Infinite(무한) , 숫자를 적음
  - play-state : 애니메이션의 재생 상태, 이벤트를 통해 멈추거나 다시 재생시킬 수 있음
  - timing-function : 진행 속도를 추가 설정 점점 빠르게 등등(ease,linear,ease-in 등)
  - fill mode : 재생 전 후의 상태를 지정
    - none : 기본 값. 재생 중이 아닌 경우 요소의 스타일을 유지
    - forwards : 재생 중이 아닌 경우 마지막 키프레임 상태를 유지
    - backwords : 재생 중이 아닌 경우 첫번째 키프레임 상태 유지

```css
@keyframe ball {
  /* 0%, 100% 는 from to를 이용해서 사용 가능 */
  0% {
    top: 0px;
    background-color: #fff;
  }

  100% {
    top: 300px;
    width: 115px;
    height: 90px;
    /* translate는 위치 이동 거리 */
    /* transform : rotate(deg)를 이용해서 회전도 시킬 수 있음 */
    transform: translate(100px, 100px);
    background-color: rgb(56, 94, 165);
  }
}

@keyframes rotate {
  0% {
    background-color: #ff23ff;
  }
  20% {
    transform: rotate(10deg);
  }
  40% {
    transform: rotate(180deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.ball1 {
  position: relative;
  display: inline-block;
  left: 100px;
  top: 0;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: blue;
  /* direction : alternate : 왔다 갔다 반복을 실행할 건지? iteration count 2이상 지정 혹은 Infinite(무한으로 지정) 
  normal 설정 시 한번 갔다 0% 지점 부터 다시 시작
  /* iteration count(애니메이션 실행 시간을 의미) : Infinite(무한으로 계속 실행), 숫자 카운트 */
  animation: ball 1s ease-in Infinite alternate;
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="/index.css" />
  </head>
  <body>
    <div class="ball1">a</div>
    <div class="rec">a</div>
  </body>
</html>
```
