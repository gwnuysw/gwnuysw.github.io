---
layout: post
title:  "react component의 life cycle API"
date:   2019-02-12 14:04:00 +0900
categories: jekyll update
comments : true
---

이 API 는 컴포넌트가 여러분의 브라우저에서 나타날때, 사라질때, 그리고 업데이트 될 때, 호출되는 API 입니다.

- 컴포넌트 초기 생성
  - constructor

    컴포넌트가 새로 만들어질 때마다 이 함수가 호출됩니다.
  - componentDidMount

    주로 D3, masonry 처럼 DOM 을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통하여 ajax 요청을 하거나, DOM 의 속성을 읽거나 직접 변경하는 작업을 진행합니다.
- 컴포넌트 업데이트

  컴포넌트가 업데이트는 props 의 변화, 그리고 state 의 변화에 따라 결정됩니다.
  - getDerivedStateFromProps

    이 API 는 props 로 받아온 값을 state 로 동기화 하는 작업을 해줘야 하는 경우에 사용됩니다.
  - shouldComponentUpdate

    Virtual DOM 에 리렌더링 하는것도, 불필요할 경우엔 방지하기 위해서 이 API를 작성합니다. 이 함수는 기본적으로 true 를 반환합니다. 우리가 따로 작성을 해주어서 조건에 따라 false 를 반환하면 해당 조건에는 render 함수를 호출하지 않습니다.
  - getSnapshotBeforeUpdate

    이 API를 통해서, DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 여기서 리턴하는 값은 componentDidUpdate 에서 3번째 파라미터로 받아올 수 있게 됩니다.

  - componentDidUpdate

    이 API는 컴포넌트에서 render() 를 호출하고난 다음에 발생하게 됩니다. 이 시점에선 this.props 와 this.state 가 바뀌어있습니다. 그리고 파라미터를 통해 이전의 값인 prevProps 와 prevState 를 조회 할 수 있습니다. 그리고, getSnapshotBeforeUpdate 에서 반환한 snapshot 값은 세번째 값으로 받아옵니다.
- 컴포넌트 제거

  - componentWillUnmount

    여기서는 주로 등록했었던 이벤트를 제거하고, 만약에 setTimeout 을 걸은것이 있다면 clearTimeout 을 통하여 제거를 합니다. 추가적으로, 외부 라이브러리를 사용한게 있고 해당 라이브러리에 dispose 기능이 있다면 여기서 호출해주시면 됩니다.

- 컴포넌트 에러 발생

  - componentDidCatch

    에러가 발생하면 이런식으로 componentDidCatch 가 실행되게 하고, state.error 를 true 로 설정하게 하고, render 함수쪽에서 이에 따라 에러를 띄워주시면 됩니다. 이 API 를 사용하시게 될 때 주의하실 점이 있는데요, 컴포넌트 자신의 render 함수에서 에러가 발생해버리는것은 잡아낼 수는 없지만, 그 대신에 컴포넌트의 자식 컴포넌트 내부에서 발생하는 에러들을 잡아낼 수 있습니다

 __reference : https://velopert.com/3631__
