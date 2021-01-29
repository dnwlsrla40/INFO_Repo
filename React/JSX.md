# JSX

JSX는 리액트에서 생김새를 정의할 때 사용하는 문법이다.

JavaScript와 HTML이 합쳐진 구조로 유일하게 react가 제공하는 독단적인 기능이다.

```
return <div>안녕하세요</div>;
```

리액트 컴포넌트 파일에서 XML 형태로 코드를 작성하면 babel이 JSX를 JavaScript로 변환

 - Babel은 JS 문법을 확장해주는 도구
 - 아직 지원되지 않는 최신 문법이나 편의상 사용하거나 실험적인 JS 문법들을 정식 JS 형태로 변환해 구형 브라우저 환경에서도 제대로 실행 할 수 있게 해주는 역할  

![Babel](/picture/babel.PNG)

 JSX가 JS로 제대로 변환되려면 지켜야 할 규칙이 있다.

 ## JSX 규칙

 ### 1. 꼭 닫혀야 하는 태그

 다음과 같은 코드는 오류가 발생하게 된다.

 ```
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <div>
      <Hello />
      <Hello />
      <Hello />
      <div>
    </div>
  );
}

export default App;
 ```

 ![꼭 닫혀야 하는 태그](/picture/닫혀야_하는_태그.PNG)

 input과 br 역시 `<input/>`, `<br/>`의 형식으로 Self Closing 태그를 사용해야 한다. 

 ### 2. 꼭 감싸져야하는 태그

 두개 이상의 태그는 하나의 태그로 감싸져있어야 한다.

 ```
 import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello />
    <div>안녕히계세요.</div>
  );
}

export default App;
 ```

![꼭 감싸져야 하는 태그](/picture/감싸져야_하는_태그.PNG)

아래 코드처럼 감싸줘야 한다.

```
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <div>
      <Hello />
      <div>안녕히계세요</div>
    </div>
  );
}

export default App;
```

하지만 이렇게 단순히 감싸기 위해서 불필요한 div를 사용하는 것은 별로 좋지 않은 상황이다. 예를 들어 스타일 관련 설정이 복잡해지게 되는 상황도 올 수 있고, table 관련 태그 역시 내용을 div 같은 걸로 감싸기엔 애매하다. 그래서 react는 Fragment라는 것을 제공한다.

```
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <>
      <Hello />
      <div>안녕히계세요</div>
    </>
  );
}

export default App;
```

태그를 작성할 때 이름 없이 작성을 하게 되면 Fragment가 만들어지는데, Fragment는 브라우저 상에서 따로 별도의 엘리먼트로 나타나지 않는다.

![Fragment](/picture/Fragment.PNG)


### 3. JSX안에 JS 값 사용하기

JSX 내부에 JS 변수를 사용할 때에는 `{}`으로 감싸서 보여줘야 한다.

```
import React from 'react';
import Hello from './Hello';

function App() {
  const name = 'react';
  return (
    <>
      <Hello />
      <div>{name}</div>
    </>
  );
}

export default App;
```

### 4. Style과 className  

**인라인 스타일**

객체 형태로 작성해야 하며 "background-color"처럼 "-"로 구분되어 있는 이름들은 "backgroundColor"처럼 camelCase 형태로 네이밍 해줘야 한다.

```
import React from 'react';
import Hello from './Hello';

function App() {
  const name = 'react';
  const style = {
    backgroundColor: 'black',
    color: 'aqua',
    fontSize: 24, // 기본 단위 px
    padding: '1rem' // 다른 단위 사용 시 문자열로 설정
  }

  return (
    <>
      <Hello />
      <div style={style}>{name}</div>
    </>
  );
}

export default App;
```

**className**

CSS class를 설정할 때에는 "class= " 가 아닌 "className= "으로 설정해야 한다.

```
App.css

.gray-box {
  background: gray;
  width: 64px;
  height: 64px;
}
```

```
App.js

import React from 'react';
import Hello from './Hello';
import './App.css';


function App() {
  const name = 'react';
  const style = {
    backgroundColor: 'black',
    color: 'aqua',
    fontSize: 24, // 기본 단위 px
    padding: '1rem' // 다른 단위 사용 시 문자열로 설정
  }

  return (
    <>
      <Hello />
      <div style={style}>{name}</div>
      <div className="gray-box"></div> // class= x, className = o
    </>
  );
}

export default App;
```

### 5. 주석

JSX에서 주석은 `{/* 이런 형태로 */}` 작성해야 한다.

```
import React from 'react';
import Hello from './Hello';
import './App.css';


function App() {
  const name = 'react';
  const style = {
    backgroundColor: 'black',
    color: 'aqua',
    fontSize: 24, // 기본 단위 px
    padding: '1rem' // 다른 단위 사용 시 문자열로 설정
  }

  return (
    <>
      {/* 주석은 화면에 보이지 않습니다 */}
      /* 중괄호로 감싸지 않으면 화면에 보입니다 */
      <Hello 
      />
      <div style={style}>{name}</div>
      <div className="gray-box"></div>
    </>
  );
}
```

추가 적으로 열린 태그 내부에는 `//`로 주석 작성이 가능하다.

```
import React from 'react';
import Hello from './Hello';
import './App.css';


function App() {
  const name = 'react';
  const style = {
    backgroundColor: 'black',
    color: 'aqua',
    fontSize: 24, // 기본 단위 px
    padding: '1rem' // 다른 단위 사용 시 문자열로 설정
  }

  return (
    <>
      {/* 주석은 화면에 보이지 않습니다 */}
      /* 중괄호로 감싸지 않으면 화면에 보입니다 */
      <Hello 
        // 열리는 태그 내부에서는 이렇게 주석을 작성 할 수 있습니다.
      />
      <div style={style}>{name}</div>
      <div className="gray-box"></div>
    </>
  );
}

export default App;
```