---
title: React Lifecycle Hooks
date: 2019-08-10 04:00:00
tags:
  - ReactJS
  - JS
  - Front-End
---

React components go through few phases during their lifecycle.
{% asset_img react-lifecycle.png mount update unmount %}
In each of these phases React give us methods "hooks" that execute at each phase.
_Notice, that I will not cover all the available hooks you can check the documentation for that I will go over the most common ones only._

## Mount

1. constructor
   The constructor is called **once** when an instance of a class is created. This where you get to initialize your properties e.g setting the initial state.
   ```js
   class Example extends Component {
     constructor(props) {
       // super is used to call parent constructor
       super(props);
     }
   }
   ```
2. componentDidMount
   This one is called **after** the component is rendered **into the DOM** and it's the **recommended** place to do AJAX calls.
   ```js
    class Example extends Component {
        ...
        componentDidMount() {
            // AJAX
            this.setState({ newDataFromAJAX });
        }
    }
   ```
3. render
   render is called every time the state changes, and it is called **after** componentDidMount as it returns to the **virtual DOM**.
   One more thing is that when a component is rendered all its children are also rendered recursively.

## Update

1. render
   render is also considered a hook of the update phase.
2. componentDidUpdate
   Called immediately after updating occurs.
   This method is not called for the initial render.
   This is a good place to do AJAX calls as long as you are comparing current props to previous props.

   ```js
   componentDidUpdate(prevProps, prevState) {
       // Notice that you must compare with previous prop before doing AJAX.
       if (this.props.userID !== prevProps.userID) {
           // do AJAX.
           this.setState({});
       }
   }
   ```

## Unmount

1. componentWillUnmount
   Called just before a component is removed from the DOM.
   use this to clean timers, or to cancel network requests.
   You should never set state in this method.
