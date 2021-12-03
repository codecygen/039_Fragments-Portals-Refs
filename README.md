# Fragments, Portals, Refs


## Fragments
Fragments is used just to put return elements together. Instead of using <div></div> you put them inside the brackets of <React.Fragment></React.Fragment> or simply <></>. This will wrap all elements but it will not add an extra layer of div element which is useful to avoid div soup inside the code that is created by React.

```
<>
    <AddUser />
    <Button />
</>
```

## Portals
Portals are used to render the element in a different place in DOM. These are the steps to follow Initially, go to "index.html" then put these lines. Notice that "root" id is the default id and then we created "backdrop-root" and "overlay-root" as ids because we will port an inner DOM element to these locations. The reason we are doing this is, we want our code to look semantically clean in some cases to prevent some errors.

```
    <div id="backdrop-root"></div>
    <div id="overlay-root"></div>
    <div id="root"></div>
```

```
import ReactDOM from 'react-dom';

///////////
///////////

const ErrorModal = (props) => {
  return (
    // Also shown as <></>
    <React.Fragment>
      {ReactDOM.createPortal(<Backdrop onConfirm={props.onConfirm} />, 
      document.getElementById("backdrop-root"))}

      {ReactDOM.createPortal(
        <ModalOverlay 
          title={props.title} 
          message={props.message} 
          onConfirm={props.onConfirm} 
        />, 
        document.getElementById("overlay-root")
      )}
    </React.Fragment>
  );
};
```

Ref hook is different than State hook. If you just want to read an element, Ref hook has less code. If you also want to re-render something, you need to use State hook.

## Refs
Refs (UseRef) are hooks just like States (UseState). You should use Refs if you don't need to re-render an element. If you need to re-render the element use State hooks. You can reset the value of the input elements with Refs as seen in the code down below but keep in mind that it is an uncontrolled component unlike in the case of using State hook for the input value control. See the code down below.

```
// AddUser.js

import React, { useState, useRef } from 'react';

import Card from '../UI/Card';
import Button from '../UI/Button';
import ErrorModal from '../UI/ErrorModal';
import classes from './AddUser.module.css';

const AddUser = (props) => {
  // Using useRef
  const nameInputRef = useRef();
  const ageInputRef = useRef();
  // Using useRef

  // const [enteredUsername, setEnteredUsername] = useState('');
  // const [enteredAge, setEnteredAge] = useState('');
  const [error, setError] = useState();

  const addUserHandler = (event) => {
    event.preventDefault();

    // Using useRef
    const enteredUsernameRef = nameInputRef.current.value;
    const enteredAgeRef = ageInputRef.current.value;
    // Using useRef

    // Using useRef
    if (enteredUsernameRef.trim().length === 0 || enteredAgeRef.trim().length === 0) {
      setError({
        title: 'Invalid input',
        message: 'Please enter a valid name and age (non-empty values).',
      });
      return;
    }

    // Using useRef
    if (+enteredAgeRef < 1) {
      setError({
        title: 'Invalid age',
        message: 'Please enter a valid age (> 0).',
      });
      return;
    }

    // Using useRef
    props.onAddUser(enteredUsernameRef, enteredAgeRef);

    // setEnteredUsername('');
    // setEnteredAge('');

    // This is uncontrolled component access.
    nameInputRef.current.value = '';
    ageInputRef.current.value = '';
  };

  // const usernameChangeHandler = (event) => {
  //   setEnteredUsername(event.target.value);
  // };

  // const ageChangeHandler = (event) => {
  //   setEnteredAge(event.target.value);
  // };

  const errorHandler = () => {
    setError(null);
  };

  return (
    <>
      {error && (
        <ErrorModal
          title={error.title}
          message={error.message}
          onConfirm={errorHandler}
        />
      )}
      <Card className={classes.input}>
        <form onSubmit={addUserHandler}>
          <label htmlFor="username">Username</label>
          <input
            id="username"
            type="text"
            // value={enteredUsername}
            // onChange={usernameChangeHandler}
            // Using useRef
            ref={nameInputRef}
          />
          <label htmlFor="age">Age (Years)</label>
          <input
            id="age"
            type="number"
            // value={enteredAge}
            // onChange={ageChangeHandler}
            // Using useRef
            ref={ageInputRef}
          />
          <Button type="submit">Add User</Button>
        </form>
      </Card>
    </>
  );
};

export default AddUser;

```