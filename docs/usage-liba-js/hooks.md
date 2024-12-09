---
sidebar_position: 4
---

# Hooks

The first hook we'll look at is useState.

As we remember, the second parameter in each functional component is an object with the key "liba".
This "liba" provides access to the useState method.

```typescript title="src/Counter.component.ts"
import {ComponentLibaParam, LocalState, RenderParams} from "types";

type Props = {
    text: string
}

export const CounterComponent = (props: Props, {liba}: ComponentLibaParam) => {
    const element = document.createElement('div');
    const [state, setState] = liba.useState(0)

    const intervalId = setInterval(() => {
        setState(prevState => prevState + 1)
    }, 1000)

    return {
        element,
        props: {...props, setState},
        localState: state
    };
}

CounterComponent.render = ({element, localState}: RenderParams<Props, LocalState<number>>) => {
    element.append(localState.value.toString());
};
```

This hook has an API similar to React, 
except that all local state is stored in the state object, under the value key.

And you can also return it from a functional component to access it in the render method and display it on the page.

:::info

Pay attention to the typing of the Local State. 
The second generic in RenderParams you must pass is a built-in type that accepts a generic 
of the type that your local state value corresponds to.
:::

You can also explicitly prototype what type your local state should store by explicitly specifying it in the generic:

```typescript title="src/Counter.component.ts"
import {ComponentLibaParam, LocalState, RenderParams} from "types";

type Props = {
    text: string
}

export const CounterComponent = (props: Props, {liba}: ComponentLibaParam) => {
    const element = document.createElement('div');
    const [state, setState] = liba.useState<string>(0)
    // Error: TS2345: Argument of type number is not assignable to parameter of type string | (() => string)

    const intervalId = setInterval(() => {
        setState(prevState => prevState + 1)
    }, 1000)

    return {
        element,
        props: {...props, setState},
        localState: state
    };
}

CounterComponent.render = ({element, localState}: RenderParams<Props, LocalState<number>>) => {
    element.append(localState.value.toString());
};
```

