## react-animations

- `npx create-react-app react-animations --template typescript`

## framer motion

- ReactJs 애니메이션 라이브러리
- `npm install framer-motion`

### framer motion 사용

```javascript
import { motion } from "framer-motion";

function App() {
  return (
    <Wrapper>
      <Box />
      <div></div> // div 로는 사용할 수 없다.
      <motion.div></motion.div> // motion.@@@ 식으로 framer motion 을 사용한다.
    </Wrapper>
  );
}
```

- 애니메이트 하려면 html 요소를 불러와야 한다 : `<motion.span></motion.span>`

### cracko

- create-react-app 의 설정사항을 override 할 수 있게 해준다.
- cracko 설치: `npm i @craco/craco --save`

### style component 를 애니메이트 하는 방법

- 애니메이트된 스타일 컴포넌트를 어떻게 가질 수 있을까?
- `styled.div` => `styled(motion.div)`
  - motion div 를 만들 수 있게 된다.
  - 스타일을 motion div 에 덧붙이게 된다.

```javascript
// 다음 대신에
const Box = styled.div`
  width: 200px;
  height: 200px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

// styled(motion.div) 를 사용
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  return (
    <Wrapper>
      <Box />
      <motion.div></motion.div>
    </Wrapper>
  );
}
```

#### animate prop

- prop 을 넣는 것만으로 애니메이션을 만들 수 있다.

```javascript
function App() {
  return (
    <Wrapper>
      <Box animate={{ borderRadius: "100px" }} />
      <motion.div></motion.div>
    </Wrapper>
  );
}
```

#### transition prop

- https://www.framer.com/docs/transition/
- css 에서 작업하지 않고 prop 에서도 transition 을 넣을 수 있다.

```javascript
function App() {
  return (
    <Wrapper>
      <Box transition={{ duration: 3 }} animate={{ borderRadius: "100px" }} />
      <motion.div></motion.div>
    </Wrapper>
  );
}
```

#### initial prop

- 애니메이션을 시작하는 방법을 명시한다.
- 시작시 element 의 초기상태를 넣어주면 된다.

```javascript
function App() {
  return (
    <Wrapper>
      <Box initial={{ scale: 0 }} animate={{ scale: 1, rotateZ: 360 }} />
    </Wrapper>
  );
}
```

#### 참고

- 모든 에니매이션에는 트랜지션 타입에 `spring` 이라는 게 디폴트로 달려 있어서 살짝 튕기는 효과가 있다
- `type`은 `spring`, `tween`, `keyframes`, `just`, `inertia` 등이 있다.
- spring 타입은 실제 물리현상을 시뮬레이팅 한다.
  - damping, stiffness 등의 옵션을 추가해서 커스텀 할 수 있다.
  - `transition = {{ type: "spring" , stiffness: 10 }}`

```javascript
function App() {
  return (
    <Wrapper>
      <Box transition={{ type: "spring" }} /> // 디폴트 설정
    </Wrapper>
  );
}
```

- 없애는 방법

```javascript
function App() {
  return (
    <Wrapper>
      <Box transition={{ type: "spring", delay: 0.5 }} /> // 디폴트 설정
    </Wrapper>
  );
}
```

## variants

-variant 는 애니메이션의 stage 를 말한다. 예를들어 어떤 stage에 initial 이 있을 수 있고, 다른 stage에는 finish, 다른 stage에는 showing, hidden, from..to.. , 0%, 50%, 100% 등 원하는 것이 될 수 있다.

- variants 는 코드를 깔끔하게 해준다. (prop을 일일이 적을 필요 없이 미리 정의해준 이름만 적어주기 때문)

```javascript
// variants 적용 전
function App() {
  return (
    <Wrapper>
      <Box
        transition={{ type: "spring", delay: 0.5 }}
        initial={{ scale: 0 }}
        animate={{ scale: 1, rotateZ: 360 }}
      />
    </Wrapper>
  );
}

// variants 적용 후
const myVars = {
  // 우선 variants 오브젝트를 정의해준다.
  start: { scale: 0 },
  end: { scale: 1, rotateZ: 360, transition: { type: "spring", delay: 0.5 } },
};
function App() {
  return (
    <Wrapper>
      // variants 를 적용한다.
      <Box variants={myVars} initial='start' animate='end' />
    </Wrapper>
  );
}
```

- 많은 애니메이션을 하나로 연결시켜준다.

#### Note

- motion 은 부모의 animate 를 자식요소에 복사하는 것이 디폴트로 되어있다. 부모의 ㅇ

```javascript
// 스타일 설정
const Wrapper = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;
const Circle = styled(motion.div)`
  background-color: white;
  place-self: center;
  height: 70px;
  width: 70px;
  border-radius: 35px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;
```

#### 방법1. 자식요소의 variants 에 `delay`를 넣어준다.

```javascript
const boxVariants = {
  start: { opacity: 0, scale: 0.5 },
  end: {
    opacity: 1,
    scale: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
    },
  },
};
const circleVariants = {
  start: {
    opacity: 0,
  },
  end: {
    opacity: 1,
    transition: {
      type: "spring",
      delay: 0.5, // 부모요소 duration이 0.5 니까 그만큼 딜레이시킨 후 애니메이트 작동
    },
  },
};
function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial='start' animate='end'>
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
```

#### 방법2. `delayChildren`

```javascript
const boxVariants = {
  start: { opacity: 0, scale: 0.5 },
  end: {
    opacity: 1,
    scale: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
      delayChildren: 0.5, // 자식요소(circle)이 0.5초 뒤에 애니메이트 시작하게 한다.
    },
  },
};
const circleVariants = {
  start: {
    opacity: 0,
  },
  end: {
    opacity: 1,
    transition: {
      type: "spring",
    },
  },
};
function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial='start' animate='end'>
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
```

#### 방법3: staggerChildren

- staggerChildren 을 box에 설정해주면, 자동적으로 첫번째 circle 에 0.5s delay를 주고 , 두번째에 1.0s , 세번째 1.5s ,.. 식으로 자동으로 들어간다.

```javascript
const boxVariants = {
  start: { opacity: 0, scale: 0.5 },
  end: {
    opacity: 1,
    scale: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
      staggerChildren: 0.5, // 0.5s, 1.0s, 1.5s, 2.0s
    },
  },
};
const circleVariants = {
  start: {
    opacity: 0,
  },
  end: {
    opacity: 1,
    transition: {
      type: "spring",
    },
  },
};
function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial='start' animate='end'>
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
```

##

### prop: while블라블라

- `whileDrag` 드래그 하는 동안 애니메이션
- `whileFocus`
- `whileHover` 마우스를 올리면 애니메이션
- `whileInView`
- `whileTap` 클릭하면 애니메이션

```javascript
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  return (
    <Wrapper>
      <Box
        whileHover={{ scale: 1.5, rotateZ: 90 }}
        whileTap={{ scale: 1, borderRadius: "100px" }}
      />
    </Wrapper>
  );
}
```

#### variants 도 같이 써보기

```javascript
const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
};

function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} whileHover={"hover"} whileTap={"click"} />
    </Wrapper>
  );
}
```

## drag 기능 구현하기

- `drag` prop 만 추가하면 된다.

```javascript
function App() {
  return (
    <Wrapper>
      <Box drag />
    </Wrapper>
  );
}
```

#### 색상 변경 animate

- `whileDrag = {{backgroundColor: "blue}}` 로하면 색상이 `string` 이기 때문에 드래그 하자마자 blue로 바뀌는 문제 -> rgb 값으로 숫자를 넣어주면 자연스럽게 transition 할 수 있다.

```javascript
function App() {
  return (
    <Wrapper>
      <Box drag whileDrag={{ backgroundColor: "rgb(46,204,113)" }} />
    </Wrapper>
  );
}
```

#### drag 에 제약기능 넣기

- `<Box drag />` : 어느 방향으로나 드래그 가능. 제약 없음
- `<Box drag='x' />` : 좌우(x좌표)로만 이동시키기
- `<Box drag dragConstraints={{top:-50, bottom:50, left:-50, right: 50}} />` : 드래그 가능한 영역 정해주기 (제약을 넘어간 곳에서 놓을시 제자리로 돌아감)
- - `<Box drag dragConstraints={{top:-50, bottom:50, left:-50, right: 50}} />` : 드래그를 놓으면 원래 자리로 돌아감

#### drag 박스를 박스안에 넣어서 제약하기

1. 방법1. BiggerBox 를 만들어서 Box를 감싸고, dragConstraints를 정해준다.

```javascript
const BiggerBox = styled.div`
  width: 600px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
`;
<BiggerBox>
  <Box
    drag
    dragConstraints={{ top: -200, bottom: 200, left: -200, right: 200 }}
    variants={boxVariants}
    whileHover={"hover"}
    whileTap={"click"}
    whileDrag={"drag"}
  />
</BiggerBox>;
```

- 추천방법: `ref` 를 사용한다. (코드로 특정 element 를 잡아줌)
- 부모 박스에 ref 를 만들어주고, 자식박스에 dragConstraints로 ref를 넣어준다.

```javascript
import { useRef } from "react";

function App() {
  const biggerBoxRef = useRef < HTMLDivElement > null;
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragConstraints={biggerBoxRef}
          variants={boxVariants}
          whileDrag={"drag"}
        />
      </BiggerBox>
    </Wrapper>
  );
}
```

#### `dragSnapToOrigin` 드래그 풀면 처음 위치로 돌아가게 하기

- `dragSnapToOrigin` prop 을 넣어주면 드래그를 풀때 처음 위치로 돌아간다.

```javascript
import { useRef } from "react";

function App() {
  const biggerBoxRef = useRef < HTMLDivElement > null;
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragSnapToOrigin
          dragConstraints={biggerBoxRef}
          variants={boxVariants}
          whileDrag={"drag"}
        />
      </BiggerBox>
    </Wrapper>
  );
}
```

#### prop: `dragElastic`

- `dragElastic` 의 값은 0~1 사이 (기본값은 0.5)
- 드래그시 마우스와 박스가 고무줄처럼 움직이게 하는 값이다.
- 1 값이면 constraint 영역을 넘어가도 박스와 마우스가 정확히 일치해서 움직인다.
- 0 값이면 constraint 된 값 안에서 정확히 머무르며 마우스와 박스가 일치해 있다.

```javascript
function App() {
  const biggerBoxRef = useRef < HTMLDivElement > null;
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragSnapToOrigin
          dragElastic={0.7}
          dragConstraints={biggerBoxRef}
          variants={boxVariants}
          whileDrag={"drag"}
        />
      </BiggerBox>
    </Wrapper>
  );
}
```

## 8.7 motionValues

- 애니메이션 내의 수치를 트래킹 할때 쓴다.
- motionvalue 가 업데이트 될 때 react rendering cycle을 발동시키지 않는다. (state 가 아니기 때문)
- 예시: 유저가 어느 방향으로 드래깅 하는지 알고 싶을 때

```javascript
import {motion, useMotionValue} from "framer-motion"

// motion.div 의 x좌표를 추적

export function MyComponent(){
  const x = useMotionValue(0)
  return <motion.div style = {{ x }}>
}
```

- 마우스로 드래그해서 사이즈 바꾸기

```javascript
import { motion, useMotionValue, useTransform } from "framer-motion";

function App() {
  const x = useMotionValue(0);
  const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);

  return (
    <Wrapper>
      <BiggerBox>
        <Box style={{ x, scale }} drag='x' dragSnapToOrigin />
      </BiggerBox>
    </Wrapper>
  );
}
```

```javascript
// motion 으로 스타일을 변경하려면 motion.div 를 넣어야된다.
const Wrapper = styled(motion.div)`
  height: 200vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;
const BiggerBox = styled.div`
  width: 600px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46,204,113)" },
};

function App() {
  const x = useMotionValue(0);
  // x : [-800,800] ->  [-360,360] 리니어맵
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  // 스크롤을 리스닝 한다.
  import { useViewportScroll } from "framer-motion";
  const { scrollYProgress } = useViewportScroll();
  const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);

  return (
    // 스크롤 하면 색상 변경, 드래그시 회전, 크기 변경
    <Wrapper style={{ background: gradient }}>
      <Box style={{ x, rotateZ, scale }} drag='x' dragSnapToOrigin />
    </Wrapper>
  );
}
```

#### useViewportScroll() 함수

- 스크롤을 리스닝 할 때 사용한다.
- `{scrollX, scrollY, scrollXProgress, scrollYProgress }` 오브젝트를 반환한다.
- `scrollX, scrollY` 는 픽셀 단위의 거리
- `scrollXProgress, scrollYProgress` 는 0~1사이의 값

## AnimatePresence

- `AnimatePresence` 컴포넌트는 react js app 에서 사라지는 컴포넌트를 애니메이트 한다.
- `AnimatePresence` 컴포넌트는 자식이 사라지거나(null), 나타날 때 애니메이션 효과를 일으키는 컴포넌트이다.
- 오브젝트를 랜더링하거나 없앨때 애니메이트 하고 싶을 때 이 컴포넌트를 사용한다.
- `visible` 이어야 한다.
- `조건문 children` 이 있어야 한다. (보여주거나 보여주지 않을 조건)

```javascript
//잘못된 방식. (항상 Visible이어야 하므로)
{
  showing ? (
    <AnimatePresence>
      <Box />
    </AnimatePresence>
  ) : null;
}

// 옳은 방식
<AnimatePresence>{showing ? <Box /> : null}</AnimatePresence>;
```

```javascript
// App.tsx
import styled from "styled-components";
import { motion, AnimatePresence } from "framer-motion";
import { useState } from "react";

const Wrapper = styled(motion.div)`
  height: 200vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 400px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  position: absolute;
  top: 100px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  initial: {
    opacity: 0,
    scale: 0,
  },
  visible: {
    opacity: 1,
    scale: 1,
    rotateZ: 360,
  },
  leaving: {
    opacity: 0,
    scale: 0,
    y: 50,
  },
};

function App() {
  const [showing, setShowing] = useState(false);
  const toggleShowing = () => setShowing((prev) => !prev);

  return (
    <Wrapper>
      <button onClick={toggleShowing}>Click</button>
      <AnimatePresence>
        {showing ? (
          <Box
            variants={boxVariants}
            initial='initial'
            animate='visible'
            exit='leaving'
          />
        ) : null}
      </AnimatePresence>
    </Wrapper>
  );
}
```

#### `exit` state

- exit 에 설정한 style 은 해당 element 가 어떤 애니메이셔을 발생시킬지 정한다.

```javascript

```

#### AnimatePresence 컴포넌트로 슬라이더 만들기

1. 박스들을 만든다

```javascript
function App() {
  const [visible, setVisible] = useState(1);
  const nextPlease = () => setVisible((prev) => (prev === 10 ? 10 : prev + 1));
  const prevPlease = () => setVisible((prev) => (prev === 1 ? 1 : prev - 1));
  return (
    <Wrapper>
      <AnimatePresence>
        {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map((i) =>
          i === visible ? ( // visible 과 i값이 일치하는 박스만 보여준다.
            <Box key={i}>{i}</Box>
          ) : null
        )}
      </AnimatePresence>
      <button onClick={nextPlease}>next</button>
      <button onClick={prevPlease}>prev</button>
    </Wrapper>
  );
}
```

2. 애니메이션을 추가해준다.

```javascript
const box = {
  invisible: {
    x: 500,
    opacity: 0,
    scale: 0,
  },
  visible: {
    x: 0,
    opacity: 1,
    scale: 1,
    transition: {
      duration: 1,
    },
  },
  exit: { x: -500, opacity: 0, scale: 0, transition: { duration: 1 } },
};

// 박스 prop 수정
<AnimatePresence>
  {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map((i) =>
    i === visible ? (
      <Box
        variants={box}
        initial='invisible'
        animate='visible'
        exit='exit'
        key={i}>
        {i}
      </Box>
    ) : null
  )}
</AnimatePresence>;
```

#### key

- 리액트는 key 를 통해 각 box 가 고유하다고 생각한다.
- 만약 key 를 바꾸면 react js 는 이전 element 가 없어지고 새로운 element 가 생겨났다고 생각한다.
- 이것을 슬라이더에 적용하면, key를 바꿈으로써 react js는 컴포넌트를 re-render 하고 `AnimatePresence` 는 새로 생기고 없어지는 것에 애니메이트를 구현하게 된다.

```javascript
const [visible, setVisible] = useState(1);
const nextPlease = () => setVisible((prev) => (prev === 10 ? 10 : prev + 1));
const prevPlease = () => setVisible((prev) => (prev === 1 ? 1 : prev - 1));

<AnimatePresence>
  // key= {i} 대신 key={visible}
  // visible 을 바꾸면 박스가 없어지고 새로 생겨나는 것이 되고
  <Box
    variants={box}
    initial='invisible'
    animate='visible'
    exit='exit'
    key={visible}>
    {visible}
  </Box>
</AnimatePresence>;
```

#### 슬라이더 prev 기능 구현하기

- 효과를 구현할 방향에 대해서 invisible 과 exit을 바꾸면 된다.
- `custom` prop 을 사용하자!
- custom 에는 무엇이든 쓸 수 있다.
- custom 으로 variants 를 바꿀 수 있다 : variant 의 value 를 객체를 리턴하는 함수로 바꿔야 한다.
- custom 의 prop 으로부터 variant 의 value 의 인자로 전달된다.

```javascript
const box = {
  entry: (isBack: boolean) => ({
    // 이 argument 는 custom = {back} 프롭에서 온다. 값에 따라 방향이 정해짐
    x: isBack ? -500 : 500,
    opacity: 0,
    scale: 0,
  }),
  center: {
    x: 0,
    opacity: 1,
    scale: 1,
    transition: {
      duration: 1,
    },
  },
  exit: (back: boolean) => ({
    x: isBack ? 500 : -500,
    opacity: 0,
    scale: 0,
    transition: { duration: 1 },
  }),
};

function App() {
  const [visible, setVisible] = useState(1);
  const [back, setBack] = useState(false);
  // 버튼으로 back 스테이트의 값을 바꾸고, back 의 값을 custom 프롭을 통해 variant로 전달을 하고, 원하는 애니메이트를 한다.
  const nextPlease = () => {
    setBack(false);
    setVisible((prev) => (prev === 10 ? 10 : prev + 1));
  };
  const prevPlease = () => {
    setBack(true);
    setVisible((prev) => (prev === 1 ? 1 : prev - 1));
  };

  return (
    <Wrapper>
      <AnimatePresence custom={back}>
        // motion 의 규칙에 따라 여기에도 custom={back} 을 넣어야 한다.
        <Box
          custom={back} // 이 prop 이 variant 로 전달된다.
          variants={box}
          initial='entry'
          animate='center'
          exit='exit'
          key={visible}>
          {visible}
        </Box>
      </AnimatePresence>
      <button onClick={nextPlease}>next</button>
      <button onClick={prevPlease}>prev</button>
    </Wrapper>
  );
}
```

#### `exitBeforeEnter`

- 기본적으로 이전 요소가 사라지면서(exit) 다음 요소가 생겨나는(initial) 애니메이션이 동시에 실행되는데, `exitBeforeEnter` 프롭을 `AnimatePresence` 에 추가하면 exit 이 끝난 이후에 다음 inital 이 실행된다.

```javascript
<Wrapper>
  <AnimatePresence custom={back} exitBeforeEnter>
    <Box
      custom={back}
      variants={box}
      initial='entry'
      animate='center'
      exit='exit'
      key={visible}>
      {visible}
    </Box>
  </AnimatePresence>
  <button onClick={nextPlease}>next</button>
  <button onClick={prevPlease}>prev</button>
</Wrapper>
```

## 8.14 layout 프롭

- `layout` 프롭을 제공하면, 레이아웃이 변경될때마다 애니메이션이 적용된다. (css가 변경되는 것들에 대해 자연스럽게 애니메이션이 적용됨)

```javascript
<Wrapper onClick={toggleClicked}>
  <Box
    style={{
      justifyContent: clicked ? "center" : "flex-start",
      alignItems: clicked ? "center" : "flex-start",
    }}>
    <Circle layout />
  </Box>
</Wrapper>
```

### shared layout : `layoutId` 프롭

- 서로다른 컴포넌트가 `layoutId` 값을 공유 하면, 두 요소를 동일한 것으로 취급해서 애니메이트를 적용한다.

```javascript
<Wrapper onClick={toggleClicked}>
  <Box>
    {!clicked ? (
      <Circle layoutId='circle' style={{ borderRadius: 50 }} />
    ) : null}
  </Box>
  <Box>
    {clicked ? (
      <Circle layoutId='circle' style={{ borderRadius: 0, scale: 2 }} />
    ) : null}
  </Box>
</Wrapper>
```
