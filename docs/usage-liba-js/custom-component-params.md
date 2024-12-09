---
sidebar_position: 3
---

# Custom Component Parameters

Two parameters will be sent to each component that you create yourself: props and an object with "liba".
Let's look at this with Сounter from the previous example.
```typescript title="src/Counter.component.ts"
import {RenderParams} from "types";

export const CounterComponent = () => {
    const element = document.createElement('div');

    return {
        element,
    };
}

CounterComponent.render = ({element}: RenderParams) => {
    element.append('Counter render inside App!');
};
```
