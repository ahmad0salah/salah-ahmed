---
title: Composing ReactJS Components
date: 2019-08-09 04:17:16
tags:
  - ReactJS
  - JS
  - Front-End
---

When starting with React, I thought I should create components for everything, and nest them just like HTML.
This lead to deeply nested structures which is ugly, and passing props many levels down, and
most of the components in the middle did not need this data.
To resolve this, we will need to understand how to compose our components.

## Passing Data

Components are like JavaScript functions.
They accept arbitrary inputs (props) and return React elements describing what should appear on the screen.

for example, you want to display a list of users you might compose this piece of UI as UsersList and User component.

```js
class UsersList extends Component {
    ...
    render() {
      ...
      return(
        this.state.users.map(user => <User name={user.name} age={user.age} />)
      );
    }
  }
```

## Passing Children

React gives you this special prop called children,
and we use it to pass something between the opening and closing tags of an element.

A practical application of this might be a dialog box,
where you want the consumer of that component to pass content to the dialog box.

```js
<Dialog>
  <h4> Dialog Title </h4>
</Dialog>

// to access children inside Dialog

function Dialog(props) {
  return (
    ...
    {props.children} // this will render all children passed to this component
    ...
  );
}
```

## Raising Events

Now, I want to add a delete button to the UsersList example.
In situations like this,
where you have a parent responsible for rendering children depending on their state.
You should follow the rule that states that
_the component that, **owns** a piece of state is the one **modifying** it._

{% asset_img components-signaling.png user component sends signal to users list component %}

in code this will be

```js
  class UsersList extends Component {
    ...
    handleDelete = () => {
      // this can be you sending ajax request
      // or just removing the element from the state
    }

    render() {
      ...
      return(
        this.state.users.map(
          user => <User name={user.name} age={user.age} onDelete={this.handleDelete}/>
        )
      );
    }
  }

  function User(props) {
  return (
    ...
      <button className="btn btn-danger" onClick={props.onDelete}>Delete</button>
    ...
  );
}
```

## Controlled Components

Often, you find yourself in a situation where you initialize the state of a child via props, and then you go on and start adding state to this component which is ok, but generally, it is better to have a single source of truth.

In this case, consider controlled components. A controlled component is a component that doesn't have local state it receives all data via props and raises events whenever data needs to be changed.

{% asset_img controlled-components.png controlled components receives data and sends events %}

usually, tutorials use text input as an example for such components but assume you have a shopping cart where user can increase the number of each item they are buying.
You might be tempted to add local state for each of these products counters but what if you wanted to add reset all button?
A better solution might be to make these counter a controlled component and but all the state in the parent element.

```js
class Chart extends Component{

  state = {
    items: [
      {id:45, productTitle:'', orderedAmount: 1},
      {id:75, productTitle:'', orderedAmount: 2},
      {id:36, productTitle:'', orderedAmount: 1},
    ]
  }

  handleIncrease = (item) => {
    // spread operator will clone the items stored in the current state
    const items = [...this.state.items]

    items[previousItems.indexOf(item)] = {...item};

    items.orderedAmount+++;

    this.setState({items});
   }
  handleDecrease = (item) => { /* decrease a given counter count */ }

  handleItemDelete = (item) => { /* deletes item from cart */ }

  handleReset = () => { /* resets all items counters to 0*/ }

  render() {
    return(
      ...
      {
        this.state.items.map( item => (
          <Item item={item}
            onIncrease={this.handleIncrease}
            onDecrease={this.handleDecrease}
            onDelete={this.handleItemDelete}/>
        ))
      }
    );
  }
}
```

# Destructuring

Destructuring allows us to unpack values from arrays, or properties from objects, into distinct variables. [Read more](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## References

- https://reactjs.org/docs/composition-vs-inheritance.html
