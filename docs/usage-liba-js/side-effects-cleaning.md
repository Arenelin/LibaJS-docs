---
sidebar_position: 5
---

# Side Effects Cleaning

In the counter example on the previous page, 
you may have noticed that we were left with an uncleared interval side effect.

To clean up such side effects, you need to return a cleanup function from the functional component in the object, 
into which you pass the logic for cleaning up the side effect.

This function will be called when your component is unmounted.

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
        localState: state,
        cleanup: () => {
            clearInterval(intervalId)
        }
    };
}

CounterComponent.render = ({element, localState}: RenderParams<Props, LocalState<number>>) => {
    element.append(localState.value.toString());
};
```

