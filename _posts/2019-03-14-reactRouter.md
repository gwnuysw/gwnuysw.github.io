---
layout: post
title:  "react router 사용하기"
date:   2019-03-14 1:06:00 +0900
categories: jekyll update
comments : true
---

# Path

```
import React, { Component } from 'react';
import { Route } from 'react-router-dom';
import { Home, About } from 'pages';


class App extends Component {
    render() {
        return (
            <div>
                <Route exact path="/" component={Home}/>
                <Route path="/about" component={About}/>
            </div>
        );
    }
}

export default App;
```

리액트 라우터의 path 기본적으로 중복이 된다. 따라서 위 코드의 path "/", "/about"가 있는 Route 컴포넌트 두개 모두 렌더링 된다. (/about에 / 포함) 이를 막기위해 exact를 써준다.

# Url 파라미터

```
import React, { Component } from 'react';
import { Route } from 'react-router-dom';
import { Home, About } from 'pages';

class App extends Component {
    render() {
        return (
            <div>
                <Route exact path="/" component={Home}/>
                <Route path="/about" component={About}/>
                <Route path="/about/:name" component={About}/>
            </div>
        );
    }
}

export default App;
```
여기에서 "/about/:name"의 parameter name은 하위 컴포넌트에서
```
import React from 'react';

const About = ({match}) => {
    return (
        <div>
            <h2>About {match.params.name}</h2>
        </div>
    );
};

export default About;
```
이런식으로 조회 가능하다.

# Link 컴포넌트

```
import React from 'react';
import { Link } from 'react-router-dom';

const Menu = () => {
    return (
        <div>
            <ul>
                <li><Link to="/">Home</Link></li>
                <li><Link to="/about">About</Link></li>
                <li><Link to="/about/foo">About Foo</Link></li>
            </ul>
            <hr/>
        </div>
    );
};

export default Menu;
```
이렇게하면 페이지를 새로고침 하지않고 라우트 이동이 가능하다.
