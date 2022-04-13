# Holiday List
This kata is aimed to make a developer use `fetch` inside react to get a response from a public API. For this assignment create a new project by the name `weekend holiday`, and upload it to your git.

### Learning Outcomes
By the end of this lesson, you should be able to:

- Describe the purpose of `state` in a React component (reading material)
- Explain the importance of using `setState()` instead of mutating state directly (reading material)
- Learn to call API using `fetch` (practice)
- Realise if mahaShivratri is a Holiday (exercise/kata)

### State

One of the main pillar of React is `state`. State is simply what we use to handle values that can change over time. For example, consider a very simple application that has a button and a counter. When the user clicks the button, the counter is incremented by 1. Since `count` will need to change on every click, we want to hold that value in `state`.

The following example of our simple counter app shows how to define `state` in React:

~~~javascript
import React, { Component } from 'react';

class App extends Component {
  constructor() {
    super();

    this.state = {
      count: 0,
    };

    this.countUp = this.countUp.bind(this);
  }

  countUp() {
    this.setState({
      count: this.state.count + 1,
    });
  }

  render() {
    return (
      <div>
        <button onClick={this.countUp}>Click Me!</button>
        <p>{this.state.count}</p>
      </div>
    );
  }
}
~~~

In the above component, we declared our state as an object with a property `count` set to an initial value of `0`. You **always** declare state in the constructor of a class component. Once again, this will work differently when we cover functional components later. The `setState` method we call inside the `countUp` method sets the state to a new value.

**IMPORTANT:** In React, state should be treated as **immutable**. This means you should **never** change state directly (i.e. without using `setState`) because it can lead to unexpected behavior or bugs.

In other words, you should never do something like: `this.state.count = 3`, or, `this.state.count++`. Instead, always use the [setState](https://reactjs.org/docs/react-component.html#setstate) method React provides to class components to modify the state. Keep this in mind - it can save you a lot of debugging when you are getting started with React. [This article](http://web.archive.org/web/20211101150139/https://lorenstewart.me/2017/01/22/javascript-array-methods-mutating-vs-non-mutating/) does a great job analyzing many popular JavaScript methods concerning mutability. Take some time to read it so you can understand how easy it can be to accidentally mutate state.

As we mentioned before, our `countUp()` method needs to be bound in our constructor (using `bind`), so it knows what context to operate in. This is a result of how `this` works in JavaScript, see [this article](https://www.freecodecamp.org/news/this-is-why-we-need-to-bind-event-handlers-in-class-components-in-react-f7ea1a6f93eb/) for a great explanation on why _this_ is the case.

In the `render` method, we access the current state through `this.state.count`. This syntax should look familiar to you by now because it is the same way we accessed props. And yes, you can also destructure state.

### API Calling

Consuming public services is an important part in becoming a React Developer. We would try to call an API, using `fetch` inside `react` and print the results as a list. The idea is to check is MahaShivratri is in the HolidayList for 2021 Indian Calendar.

A sample of how to call an API is shown below, we can try to copy/paste this inside our ```App.Js``` to visualise.

~~~javascript
import { useState, useEffect } from "react";

export default function App() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/posts?_limit=8`)
      .then((response) => {
        if (!response.ok) {
          throw new Error(
            `This is an HTTP error: The status is ${response.status}`
          );
        }
        return response.json();
      })
      .then((actualData) => {
        setData(actualData);
        setError(null);
      })
      .catch((err) => {
        setError(err.message);
        setData(null);
      })
      .finally(() => {
        setLoading(false);
      });
  }, []);

  return (
    <div className="App">
      <h1>API Posts</h1>
      {loading && <div>A moment please...</div>}
      {error && (
        <div>{`There is a problem fetching the post data - ${error}`}</div>
      )}
      <ul>
        {data &&
          data.map(({ id, title }) => (
            <li key={id}>
              <h3>{title}</h3>
            </li>
          ))}
      </ul>
    </div>
  );
}
~~~

API used here is a public API 
`https://jsonplaceholder.typicode.com/posts?_limit=8`
Try to run this API on web to understand the response.

## Holiday API

`https://holidayapi.com/v1/holidays?pretty&key=b3ea9900-0e5c-4114-9743-e7413661b260&country=IN&year=2021`

We can try to understand the response of this API and make the changes to the above code accordingly, For this exercise, we are allowed to start the development with correct the test cases as single line comments, as we don't know how to write test cases in react for now. (Write possible test cases as JS comments using `//`).

Commit the code, and enjoy your holiday.
