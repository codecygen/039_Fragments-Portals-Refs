# Fragments, Portals, Refs


## Fragments
Fragments is used just to put return elements together. Instead of using <div></div> you put them inside the brackets of <React.Fragment></React.Fragment> or simply <></>

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