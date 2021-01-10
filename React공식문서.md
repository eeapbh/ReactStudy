# React 

```jsx
const element = <h1>Hello, word!</h1>
```

> JSX라 하며 JavaScript를 확장한 문법이다
>
> UI가 어떻게 생겨야 하는지 설명하기 위해 React와 함께 사용할것을 권장한다
>
> JavaScript의 모든기능이 포함되어 있다

- JSX는 React `엘리먼트`를 생성한다. 

---



## JSX 소개

#### JSX란?

- React에서는 

  - 이벤트가 처리되는 방식,	

  -  시간에 따라 state가 변하는 방식, 

  - 화면에 표시하기 위해 데이터가 준비되는 방식 등 

    ->렌더링 로직이 본질적으로 다른 UI로직과 연결되다는 사실을 받아들인다.

- React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하지만 둘다 
  포함하는 `컴포넌트 ` 라고 부르는 느슨하게 연결된 유닛으로 관심사를 분리한다 

- React는 JSX 사용이 필수가 아니지만 대부분의 사람은 JavaScript 코드안에서 UI관련 작업을
  할때 시각적으로 더 도움이 된다고 생각한다

- React가 더욱 도움이 되는 에러및 경고 메시지를 표시할 수 있게 해준다.



#### JSX에 표현식 포함하기

##### 아래 예시에서 `name`이라는 변수를 선언한후 중괄호로 감싸 JSX안에 사용하엿다

```jsx
const name = 'John'
const element = <h1>Hello, {name}</h1>

ReactDom.render(
    element,
    document.getElementById('root')
);
```



#### JSX도 표현식이다

- 컴파일이 끝나면, JSX표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식된다
- 즉 JSX를 `if` 구문 및 `for` loop안에 사용하고, 변수에 할당하고, 인자로서 받아들이고 함수로부터 
  반환할수잇다



#### 주의

- JSX는 HTML보다는 JavaScript에 가깝기 때문에, React DOM은 HTML 어트리뷰트 이름 대신 `camelCase` 프로퍼티 명명 규칙을 사용합니다.
- ex = className, tabIndex



#### JSX로 자식 정의

```jsx
const element = <img src={user.avatarUrl}/>;
```

> 태그가 비어있다면 XML처럼 `/>`를 이용해 바로 닫아 주어야한다

```jsx
const element = (
    <div>
        <h1>Helllo!</h1>
        <h2>Good to see you here</h2>
    </div>
);
```



#### JSX 는 주입공격을 방지한다

> JSX에 사용자 입력을 삽입하는 것은 안전하다

```jsx
const title = response.potentiallyMaliciousInput;
//이것은 안전하다
const element = <h1>{title}</h1>;
```

- ❌기본적으로 React DOM은 JSX에 삽입된 모든값을 렌더링하기 전에 이스케이프 하므로, 앱에 명시적으로 작성되지 않은 내용은 주입되지 않습니다.

- 모든항목은 렌더링 되기전에 문자열로 변환된다. 이런특성으로 인해 XSS(cross-site-scripting)공격을 방지할수있다

#### JSX는 객체를 표현한다.

---



## 엘리먼트 렌더링

> 엘리먼트는 React 앱의 가장 작은 단위이다
>
> 엘리먼트는 화면에 표시할 내용을 기술한다

```jsx
const element = <h1>Hello, world</h1>;
```

> 브라우저 DOM 엘리먼트와 달리 React 엘리먼트는 `일반객체`(plain object)이며 쉽게 생성할수 있다.
>
> React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트 한다



#### 주의

- 엘리먼트는 컴포넌트의 `"구성요소"`이다.



#### DOM에 엘리먼트 렌더링하기

- HTML 파일 어딘가에 <div>가 있다고 가정해보자

```jsx
<div id="root"></div>
```

- 이안에 들어가는 모든 엘리먼트를 React DOM에서 관리하기 때문에 이것을 "루트(root)" DOM 노드라고 부른다
- React로 구현된 앱은 일반적으로 하나의 루트 DOM 노드가 있다
- React를 기존 앱에 통합하려는 경우 원하는 만큼 많은수의 독립된 루트 DOM노드가 있을수 있다.
- React 엘리먼트를 루트 DOM노드에 렌더링하려면 둘다 `ReactDOM.render()로 전달하면 된다.

```jsx
const element = <h1>Hello, world</h1>;
React.DOM.render(elem,ent, document.getElementById('root'));
```

#### 

#### 렌더링 된 엘리먼트 업데이트 하기

- React 엘리먼트는 불변객체이다. 엘리먼트를 생성한 이후에는 해당엘리먼트의 자식이나 속성을 변경할수 없다
- 엘리먼트는 영화에서 하나의 프레임과 같이 특정 시점의 UI를 보여준다

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```



##### 변경된 부분만 업데이트 하기

- React DOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고
  DOM을 원하는 상태로 만드는데 필요한 경우에만 DOM을 업데이트 한다.
- 위에 코드는 매초 전체 UI를 다시그리도록 만들엇지만 React DOM은 내용이 변경된 텍스트 노드만
  매초 업데이트한다
- 경험에 비추어 볼 때 특정 시점에 UI가 어떻게 보일지 고민하는 이런 접근법은 시간의 변화에 따라 UI가 어떻게 변화할지 고민하는 것보다 더 많은 수의 버그를 없앨 수 있습니다.

---



## Components and Props

> 컴포넌트를 통해 UI를 재사용 가능한 개별적인 여러 조각으로 나누고, 각 조각을 개별적으로 살펴볼 수 있다.
>
> 개념적으로 컴포넌트는 JavaScript 함수와 유사하다
>
>  `"props"`라고 하는 임의의 입력을 받은후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환한다



#### 함수 컴포넌트와 클래스 컴포넌트!

###### 컴포넌트를 정의하는 가장 간단한 방법은 JavaScript 함수를 작성하는 것이다

```jsx
function Welcome(props){
    return <h1>Hello, {props.name}</h1>;
}
```

- 이 함수는 데이터를 가진 하나의 "porps"(props는 속성을 나타내는 데이터 이다)
- 객체 인자를 받은후 React 엘리먼트를 반환하므로 유효한 React 컴포넌트 이다.
- 이러한 컴포넌트는 JavaScript 함수이기 때문에 말 그대로 "함수 컴포넌트"라고 호칭한다.

###### 또한 ES6 class 를 사용하여 컴포넌트를 정의할수 있다

```jsx
class Welcome extends React.Component {
    render(){
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

- React의 관점에서 볼 때 위 두가지 유형의 컴포넌트는 동일하다
- 함수 컴포넌트와 class컴포넌트 둘다 몇가지 추가 기능이 있으며 이는 다음장에 다시 설명함



#### 컴포넌트 렌더링

- 이전까지는 React엘리먼트를 DOM태그로 나타냈다

```jsx
const element = <div/>;
```

React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼수있다

```jsx
const element = <Welcome name="Sara"/>;
```

- React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트와 자식을 해당컴포넌트에 단일객체로 전달한다
- 이 객체를 `"props"`라고 한다



###### 다음은 페이지에 "Hello Sara"를 렌더링하는 예시이다

```jsx
function Welcome(props){
    return <h1>Hello {porps.name}</h1>;
}

const element = <Welcome name="Sara"/>;
ReactDOM.render(
	element,
    document.getElementById('root')
);
```

1.  <Welcome name="Sara"/> 엘리먼트로 ReactDOM.render()를 호출한다.❌
2. React는 {name:'Sara'}를 props로 하여 Welcome 컴포넌트를 호출한다
3.  Welcome 컴포넌트는 결과적으로 <h1>Hello, Sara<h1/> 엘리먼트를 반환한다
4. ReactDOM은 <h1>Hello, Sara</h1> 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트 한다

#### 주의 : 컴포넌트의 이름은 항상 대문자로 시작

- React는 소문자로 시작하는 컴포넌트를 DOM태그로 처리한다 



#### 컴포넌트 합성

> 컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할수 있다
>
> 이는 모든 세부 단계에서 동일한 추상 컴포넌트를 사용할 수 있음을 의미한다.
>
> React앱에서는 버튼, 폼, 다이얼로그, 화면 등의 모든것들이 흔히 컴포넌트로 표현된다

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```



#### 컴포넌트 추출

> 컴포넌트를 여러개의 작은 컴포넌트로 나누는 것을 두려워하지마라!

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

- 복잡하다 😢

- 이컴포넌트는 구성요소들이 모두 중첩 구조로 이루어져 있어서 변경하기 어려울수 있고 각 구성요소를 개별적으로 재사용하기도 힘들다

###### 먼저 `Avatar`를 추출하겠다

```jsx
function Avatar(props){
    return(
    	<img className="Avatar"
            src={props.user.avatarUrl}
            alt={props.user.name}
        />
    );
}
```

- Avatar 는 자신이 Comment 내에서 렌더링 된다는 것을 알 필요가 없다
- 따라서 props의 이름을 author에서 더욱 일반화된 user로 변경하였다
- props의 이름은 사용될 context가 아닌 컴포넌트 자체의 관점에서 짓는 것을 권장한다



###### 다음으로 Avatar 옆에 사용자의 이름을 렌더링하는 UserInfo 컴포넌트를 추출하겠다

```jsx
functino UserInfo(props){
    return (
    	<div className="UserInfo">
            <Avatar user={props.user} />
            <div className="UserInfo-name">
        		{props.user.name}
      		</div>
    </div>
    )
}
```

###### Comment 가 더욱 단순해졌다

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

### 

#### props는 읽기 전용이다

###### 함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 props를 수정해서는 안된다





## State and Lifecycle

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```



-> 먼저 시계가 생긴것에 따라 캡슐화 하는 것으로 시작할수 있다

```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

-> 여기에서는 중요한 요건이 누락되어 있다.

-> Clock이 타이머를 설정하고 매초 UI를 업데이트 하는것이 Clock의 구현 세부사항이 되어야 한다

-> 이상적으로 한번만 코드를 작성하고 Clock이 스스로 업데이트 하도록 만들려고 한다



```jsx
ReactDOM.render(
	<Clock/>
    document.getElementById('root')
)
```

- 이것을 구현하기 위해서 Clock 컴포넌트에 `"sate"`를 추가 해야한다
- State는 props와 유사하지만, 비공개이며 컴포넌트에 의해 완전히 제어된다.



#### 함수에서 클래스로 변환하기

###### 다섯단계로 Clock과 같은 함수 컴포넌트를 클래스로 변환할 수 있다

1. React.Component를 확장하는 동일한 이름의 ES6 class를 생성한다.
2. render()라고 불리는 빈 메서드를 추가한다
3. 함수의 내용을 render()메서드 안으로 옮긴다
4. render()내용안에 있는 props를 this.props로 변경한다
5. 남아있는 빈 함수 선언을 삭제한다.

```jsx
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```



#### 클래스에 로컬 State 추가하기

세 단계에 걸쳐서 date를 porps에서 state로 이동해 보겠습니다

1. render()메서드 안에 있는` this.props.date  `를  `this.state.date`로 변경한다

   1. ```jsx
      class Clock extends React.Component {
        render() {
          return (
            <div>
              <h1>Hello, world!</h1>
              <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
          );
        }
      }
      ```

2. 초기 `this.state`를 지정하는 `class constructor`를 추가한다

   1. ```jsx
      class Clock extends React.Component {
        constructor(props) {
          super(props);
          this.state = {date: new Date()};
        }
      
        render() {
          return (
            <div>
              <h1>Hello, world!</h1>
              <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
          );
        }
      }
      
      ReactDOM.render(
        <Clock />,
        document.getElementById('root')
      );
      ```

   2. 클래스 컴포넌트는 항상 props로 기본 constructor를 호출해야 한다

   3. <Clock/> 요소에서 date prop 을 삭제한다

   ​	

##### 이제 Clock이 스스로 타이머를 설정하고 매초 스스로 업데이트 하도록 만들어 보겠다

#### 생명주기 메서드를 클래스에 추가하기

> 많은 컴포넌트가 있는 앱중에 컴포넌트가 삭제될대 해당 컴포넌트가 사용 중이던 리소스를 확보하는 것이 중요하다



- Clock이 처음 DOM에 렌더링 될 때 마다 타이머를 설정하려고 한다 == React에서 "마운팅"이라고 한다
- 또한 Clock에 의해 생성된 DOM이 삭제될 때 마다 타이머를 해제하려고 한다
- 컴포넌트 클래스에서 특별한 메서드를 선언하여 컴포넌트가 마운트되거나 언마운트될 때 일부 코드를 작동할 수 있다.

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
  }

  componentWillUnmount() {
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

- 이러한 메서드들은 "생명주기 메서드라고 불린다"
- componentDidMount()메서드는 컴포넌트 출력물이 DOM에 렌더링 된후에 실행된다.
- 이장소가 타이머를 설정하기에 좋은 장소이다.



```jsx
componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

- this.props 가 React에 의해 스스로 설정되고  this.state  가 특수한 의미가 있지만 타이머 ID와 같이 데이터 흐름 안에 포함되지 않는 어떤 항목을 보관할 필요가 있다면 자유롭게 클래스에 수동으로 부가적인 필드를 추가해도된다.





#### componentWillunmount()생명주기 메서드 안에 있는 타이머를 분해해 보겠다.

```jsx
componentWillUnmout(){
    clearInterval(this.timerID);
}
```



#### 마지막으로 Clock 컴포넌트가 매초 작동하도록 하는 tick()이라는 메서드를 구현해 보겠다

> 이것은 컴포넌트 로컬 state를 업데이트 하기 위해 this.setState()를 사용한다

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

- 이제 시계는 매초 째깍거립니다

1.  <Clock/> 가 ReactDOM.render()로 전달되었을 때 React는 Clock 컴포넌트의 constructor를 호출한다. Clock 이 현재 시각을 표시해야 하기 때문에 현재시각이 포함된 객체로 this.state를 초기화 한다. 나중에 이 state를 업데이트 할것이다
2. React는 Clock 컴포넌트의 render()메서드를 호출한다. 이를 통해 React는 화면에 표시되어야 할 내용을 알게 된다. 그다음 React는 Clock의 렌더링 출력값을 일치시키기 위해 DOM을 업데이트 한다
3. Clock 출력값이 DOM에 삽입되면, React는 componentDidMount()생명주기 메서드를 호출한다
   그안에서 Clock 컴포넌트는 매초 컴포넌트의 tick()메서드를 호출하기 위한 타이머를 설정하도록 브라우저에 요청한다
4. 매초 브라우저가 tick()메서드를 호출한다 그안에서 Clock 컴포넌트는 setState()에 현재 시각을 포함하는 객체를 호출하면서 UI 업데이트를 진행한다
   setState()호출 덕분에 React는 state가 변경된 것을 인지하고 화면에 표시될 내용을 알아내기 위해 render() 
   메서드를 다시 호출한다
   이 때 render()메서드 안의 this.state.date가 달라지고 렌더링 출력값은 업데이트 된 시각을 포함한다.
   React는 이에 따라 DOM을 업데이트 한다
5. Clock 컴포넌트가 DOM으로부터 한번이라도 삭제된 적이 있다면 React는 타이머를 멈추기 위해 componentWillUnmount()생명주기 메서드를 호출한다.

#### 

#### State를 올바르게 사용하기

1. 직접 State를 수정 금지

   1. 틀린코드 

      ```jsx
      // Wrong
      this.state.comment = 'Hello';
      ```

   2. 대신에 setState()를 사용한다

      ```jsx
      // Correct
      this.setState({comment: 'Hello'});
      ```

      this.state 를 지정할 수 있는 유일한 공간은 바로 `constructor`이다

2. State 업데이트는 비동기적일 수 도 있다

   1. React는 성능을 위해 여러 setState() 호출을 단일 업데이트로 한꺼번에 처리할수 있다

   2. this.props와 this.state가 비동기적으로 업데이트 될 수 있기 때문에 다음 state를 계산할때
      해당 값에 의존해서는 안된다

   3. 예를 들어 다음 코드는 카운터 업데이트에 실패할 수 있다

      ```jsx
      this.setState({
          counter:this.state.counter + this.props.increment,
      })
      ```

      -> 이를수정하기 위해 객체보다는 함수를 인자로 사용하는 다른 형태의 setState()를 사용한다
      그함수는 이전 state를 첫번째 인자로 받아들일 것이고, 업데이트가 적용된 시점의 props를 두번째
      인자로 받아들일 것이다

      ```jsx
      this.setState((state, props) => ({
          counter: state.counter + props.increment
      }))
      ```

3. State 업데이트는 병합된다

   1. setState()를 호출할 때 React는 제공한 객체를 현재 state로 병합한다

      ```jsx
       constructor(props) {
          super(props);
          this.state = {
            posts: [],
            comments: []
          };
        }
      ```

   2. 별도의 setState()호출로 이러한 변수를 독립적으로 업데이트 할 수 있다

      ```jsx
      componentDidMount() {
          fetchPosts().then(response => {
            this.setState({
              posts: response.posts
            });
          });
      
          fetchComments().then(response => {
            this.setState({
              comments: response.comments
            });
          });
        }
      ```

      병합은 얕게 이루어지기 때문에 `this.setState({comments})`는 `this.state.posts`에 영향을 주진 않지만 `this.state.comments`는 완전히 대체됩니다.

