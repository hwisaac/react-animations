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
