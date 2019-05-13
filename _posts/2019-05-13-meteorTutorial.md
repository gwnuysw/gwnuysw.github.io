---
layout: post
title:  "meteor react tutorial"
date:   2019-05-13 04:01:00 +0900
categories: jekyll update
comments : true
---

# 메테오를 이용해서 간단 todo 앱 만들기 feat. react
meteor 공식 페이지에서 제공하는 튜토리얼중에 todo튜토리얼의 react버전을 만들어 보고 그 내용을 적어봅니다.
## 디렉토리 구조
- _.meteor_(패키지 관리용이라 생략)
- *node_modules*(마찬가지로 패키지 관리용이라 생략)
- _client_
  - main.css
  - main.html
  - main.js
- _imports_
  - api
  - startup
  - ui
- _server_
 - main.js
- _tests_
  - main.js
- package.json

이탤릭체는 디렉토리입니다. 차례차례 그 내부를 들여다 보겠습니다.

### client
당연히 클라이언트 측과 관련된 내용이 있겠습니다.
#### main.css

클라이언트측에서 사용하는 스타일 시트입니다.(서버에서 사용할수도 있나?..흠..)

#### main.html

```
<head>
  <title>Linkabout</title>
</head>

<body>
  <div id="render-target"></div>
</body>
```
react가 그려질 도화지라고 생각하면 됩니다. render-target이라는 아이디가 있는 div태그 아래에 렌더링 됩니다.
### imports
client와 server에서 사용하는 기능들을 모아 놓은것 같습니다.
#### api
api는 서버에서만 사용하나 봅니다.
##### tasks.js
메테오 프로젝트를 처음 생성하면 'insecure'패키지가 자동으로 설치됩니다. 이 녀석이 있으면, 클라이언트에서 데이터 베이스로 `insert`, `update`, `remove`를 이용하여 직접적인 접근을 하는데 이는 보안상 매우 취약한 방법입니다. 취약점을 제거하기 위해 `meteor remove insecure`명령어로 패키지를 지우고 `insert`, `update`, `remove`명령어를 메소드로 래핑하여 그 안에 user가 데이터베이스를 사용할 자격이 있는지 체크하는 로직을 거친후 비로소 `insert`, `update`, `remove`명령어를 사용하도록 합니다.

```

import { Meteor } from 'meteor/meteor';
import { Mongo } from 'meteor/mongo';
import { check } from 'meteor/check';

export const Tasks = new Mongo.Collection('tasks');

if (Meteor.isServer) {
  // This code only runs on the server
  // Only publish tasks that are public or belong to the current user
  Meteor.publish('tasks', function tasksPublication() {
    return Tasks.find({
      $or: [
        { private: { $ne: true } },
        { owner: this.userId },
      ],
    });
  });
}

Meteor.methods({
  'tasks.insert'(text) {
    check(text, String);

    // Make sure the user is logged in before inserting a task
    if (! this.userId) {
      throw new Meteor.Error('not-authorized');
    }

    Tasks.insert({
      text,
      createdAt: new Date(),
      owner: this.userId,
      username: Meteor.users.findOne(this.userId).username,
    });
  },
  'tasks.remove'(taskId) {
    check(taskId, String);

    const task = Tasks.findOne(taskId);
    if (task.private && task.owner !== this.userId) {
      // If the task is private, make sure only the owner can delete it
      throw new Meteor.Error('not-authorized');
    }

    Tasks.remove(taskId);
  },
  'tasks.setChecked'(taskId, setChecked) {
    check(taskId, String);
    check(setChecked, Boolean);

    const task = Tasks.findOne(taskId);
    if (task.private && task.owner !== this.userId) {
      // If the task is private, make sure only the owner can check it off
      throw new Meteor.Error('not-authorized');
    }

    Tasks.update(taskId, { $set: { checked: setChecked } });
  },
  'tasks.setPrivate'(taskId, setToPrivate) {
    check(taskId, String);
    check(setToPrivate, Boolean);

    const task = Tasks.findOne(taskId);

    // Make sure only the task owner can make a task private
    if (task.owner !== this.userId) {
      throw new Meteor.Error('not-authorized');
    }

    Tasks.update(taskId, { $set: { private: setToPrivate } });
  },
});
```
잘 보시면 `Meteor.methods`함수 안에 여러 함수가 있는게 보이시나요? 그리고 그 안에 check와 유저확인 절차가 항상 먼저 나오고 그뒤에 DB조작명령들이 나옵니다.

또 하나 자동으로 설치되는게 있습니다. `autopublish`라고 하는 것인데 이 녀석은 서버의 데이터베이스가 통째로 클라이언트로 넘어가게 합니다. 그래서 이것도 `meteor remove autopublish`로 지워줍니다. 대신에 우리가 클라이언트로 주고싶은 데이터만 골라서 줄때는 서버쪽에서 `Meteor.publish`라고 하는 메소드를 이용하고, 클라이언트에서 `Meteor.subscribe`라고 하는 메소드를 이용해야 합니다. 그것이 바로 위 코드의 `Meteor.publish`에 대한 내용입니다.

##### tasks.tests.js
meteor에서 프로그램 테스트를 위해 mocha라고 하는 프레임워크를 사용합니다. 필요한 테스트 프레임워크를 설치합니다.
```
meteor add meteortesting:mocha
meteor npm install --save-dev chai
```
테스트를 실행할 태 사용하는 명령어 입니다.
```
TEST_WATCH=1 meteor test --driver-package meteortesting:mocha
```
원래 테스트를 실행하는 기본 엔트리 포인트는 `tests/main.js`입니다. 뭔가 테스트를 하고 싶을때 여기에 내용을 써넣으면 되지만 모듈로 만들어서 테스트 코드를 짜고 싶을 때는 `tasks.tests.js`처럼 하면 됩니다.

```
/* eslint-env mocha */

import { Meteor } from 'meteor/meteor';
import { Random } from 'meteor/random';
import { assert } from 'chai';

import { Tasks } from './tasks.js';

if (Meteor.isServer) {
  describe('Tasks', () => {
    describe('methods', () => {
      const userId = Random.id();
      let taskId;

      beforeEach(() => {
        Tasks.remove({});
        taskId = Tasks.insert({
          text: 'test task',
          createdAt: new Date(),
          owner: userId,
          username: 'tmeasday',
        });
      });

      it('can delete owned task', () => {
        // Find the internal implementation of the task method so we can
        // test it in isolation
        const deleteTask = Meteor.server.method_handlers['tasks.remove'];

        // Set up a fake method invocation that looks like what the method expects
        const invocation = { userId };

        // Run the method with `this` set to the fake invocation
        deleteTask.apply(invocation, [taskId]);

        // Verify that the method does what we expected
        assert.equal(Tasks.find().count(), 0);
      });
    });
  });
}
```
이 테스트코드는 다큐먼트를 하나 만들어서 다시 지우는 테스트를 하는군요
#### startup
렌더링 하기전에 실행되는 로직을 모아놓나봅니다.

##### accounts-config.js
메테오는 로그인 ui와 로컬로그인 전략을 패키지로 제공합니다.
```
meteor add accounts-ui accounts-password
```
그떄 다음과 같이 로그인 아이디로 그냥 유저네임을 사용할지 아니면 이메일을 사용할지 선택할 수 있습니다.
```
import { Accounts } from 'meteor/accounts-base';

Accounts.ui.config({
  passwordSignupFields: 'USERNAME_ONLY'
});
```
#### UI
리액트 컴포넌트들을 모아 놓습니다.

##### AccountsUIWrapper.js

메테오에서 기본적으로 제공하는 로그인 UI패키지는 blaze라고하는 렌더링 시스템을 이용합니다. 아쉽게도 react버전은 아직 없는것 같고, 이것을 사용하기 위해서 blaze컴포넌트를 react를 이용하여 래핑해야합니다. 이 파일은 그 래핑에 대한 내용을 담고 있습니다.
```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { Template } from 'meteor/templating';
import { Blaze } from 'meteor/blaze';

export default class AccountsUIWrapper extends Component {
  componentDidMount() {
    // Use Meteor Blaze to render login buttons
    this.view = Blaze.render(Template.loginButtons,
      ReactDOM.findDOMNode(this.refs.container));
  }
  componentWillUnmount() {
    // Clean up Blaze view
    Blaze.remove(this.view);
  }
  render() {
    // Just render a placeholder container that will be filled in
    return <span ref="container" />;
  }
}
```
##### App.js
최상위 컴포넌트 입니다.
```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { Meteor } from 'meteor/meteor';
import { withTracker } from 'meteor/react-meteor-data';

import { Tasks } from '../api/tasks.js';

import Task from './Task.js';
import AccountsUIWrapper from './AccountsUIWrapper.js';

// App component - represents the whole app
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      hideCompleted: false,
    };
  }
  handleSubmit(event) {
    event.preventDefault();
    // Find the text field via the React ref
    const text = ReactDOM.findDOMNode(this.refs.textInput).value.trim();
    Meteor.call('tasks.insert', text);
    // Clear form
    ReactDOM.findDOMNode(this.refs.textInput).value = '';
  }
  toggleHideCompleted() {
    this.setState({
      hideCompleted: !this.state.hideCompleted,
    });
  }
  renderTasks() {
    let filteredTasks = this.props.tasks;
    if (this.state.hideCompleted) {
      filteredTasks = filteredTasks.filter(task => !task.checked);
    }
    return filteredTasks.map((task) => {
      const currentUserId = this.props.currentUser && this.props.currentUser._id;
      const showPrivateButton = task.owner === currentUserId;
      return (
        <Task
          key={task._id}
          task={task}
          showPrivateButton={showPrivateButton}
        />
      );
    });
  }
  render() {
    return (
      <div className="container">
        <header>
          <h1>Todo List ({this.props.incompleteCount})</h1>
          <label className="hide-completed">
            <input
              type="checkbox"
              readOnly
              checked={this.state.hideCompleted}
              onClick={this.toggleHideCompleted.bind(this)}
            />
            Hide Completed Tasks
          </label>
          <AccountsUIWrapper />
          { this.props.currentUser ?
            <form className="new-task" onSubmit={this.handleSubmit.bind(this)} >
              <input
                type="text"
                ref="textInput"
                placeholder="Type to add new tasks"
              />
            </form> : ''
          }
        </header>
        <ul>
          {this.renderTasks()}
        </ul>
      </div>
    );
  }
}
export default withTracker(() => {
  Meteor.subscribe('tasks');
  return {
    tasks: Tasks.find({}, { sort: { createdAt: -1 } }).fetch(),
    incompleteCount: Tasks.find({ checked: { $ne: true } }).count(),
    currentUser: Meteor.user(),
  };
})(App);
```
- handleSubmit는 textInput을 찾아서 값을 읽어온후 데이터베이스에 삽입하고 textInput값을 지웁니다.
- toggleHideCompleted는 hideCompleted의 값을 반전합니다.
- renderTasks는 하위 컴포넌트인 Task컴포넌트를 만듭니다. 여기서 논리연산자와 assignment가 동시에 쓰였는데 연산자의 왼쪽이 참일 경우 연산자의 오른쪽 값이 assign되고 왼쪽이 거짓일 경우 왼쪽 값이 넘어갑니다.

맨 아래 withTracker에서 'tasks'publish 를 subscribe하고 있습니다. 그리고 이 함수가 리턴하는 값들은 App컴포넌트에서 props를 이용해서 접근할 수 있습니다.

##### Task.js

```
import React, { Component } from 'react';
import { Meteor } from 'meteor/meteor';
import classnames from 'classnames';
import { Tasks } from '../api/tasks.js';

// Task component - represents a single todo item
export default class Task extends Component {
  toggleChecked() {
    // Set the checked property to the opposite of its current value
    Meteor.call('tasks.setChecked', this.props.task._id, !this.props.task.checked);
  }

  deleteThisTask() {
    Meteor.call('tasks.remove', this.props.task._id);
  }

  togglePrivate() {
    Meteor.call('tasks.setPrivate', this.props.task._id, ! this.props.task.private);
  }
  render() {
    // Give tasks a different className when they are checked off,
    // so that we can style them nicely in CSS
    const taskClassName = classnames({
      checked: this.props.task.checked,
      private: this.props.task.private,
    });
    return (
      <li className={taskClassName}>
        <button className="delete" onClick={this.deleteThisTask.bind(this)}>
          &times;
        </button>
        <input
          type="checkbox"
          readOnly
          checked={!!this.props.task.checked}
          onClick={this.toggleChecked.bind(this)}
        />
        { this.props.showPrivateButton ? (
          <button className="toggle-private" onClick={this.togglePrivate.bind(this)}>
            { this.props.task.private ? 'Private' : 'Public' }
          </button>
        ) : ''}
        <span className="text">
          <strong>{this.props.task.username}</strong>: {this.props.task.text}
        </span>
      </li>
    );
  }
}
```
할일 하나하나 만드는 컴포넌트입니다. 신기한건 느낌표를 두번 이용하는 기법인데, 이는 값이 뭐라도 있나 없나 체크할때 쓰입니다.

### server

#### main.js

신기하게도 달랑 한줄입니다.
```
import '../imports/api/tasks.js';
```
### tests
#### main.js
main.js라는 이름의 파일이 계속해서 등장합니다. 이 파일은 앞서 설명했듯이 테스트 코드를 작성합니다. 메테오에서는 테스트코드를 작성하는데 `describe`, `it`이라고 하는 메소드를 이용합니다.
```
import assert from "assert";
import "../imports/api/tasks.tests.js";
describe("Linkabout", function () {
  it("package.json has correct name", async function () {
    const { name } = await import("../package.json");
    assert.strictEqual(name, "Linkabout");
  });

  if (Meteor.isClient) {
    it("client is not server", function () {
      assert.strictEqual(Meteor.isServer, false);
    });
  }

  if (Meteor.isServer) {
    it("server is not client", function () {
      assert.strictEqual(Meteor.isClient, false);
    });
  }
});
```

> 참조 : https://www.meteor.com/tutorials/react/creating-an-app
