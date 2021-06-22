# Typescript

## Basic Syntax

```typescript
// OR operator
null | number

// Mandatory Parameter
prop1 : String

// Optional Parameter
prop1 ?: String

// Data Types
String
Boolean
Number // Integer or Double

// React
// Functional Component
export const ComponentName: React.FC<Props> = () => {
    ...
}

// Interface for Props
interface Props {
	prop1: String; // Mandatory Prop (Only Colon)
    prop2?: Boolean // Optional Prop (Question Mark + Colon)
}
    
// Inteface for JS Object
interface Person {
    firstName: String
    lastName: String
}
```



## Defining a React Functional Component

```typescript
import React from 'react';

interface Props {
	prop1: String; // Mandatory Prop (Only Colon)
    prop2?: Boolean // Optional Prop (Question Mark + Colon)
}

// Define the type of each JS variable after the colon
// In this case It's a React Function Component
// Also Pass an interface defining the type of Props that the function needs
export const ComponentName: React.FC<Props> = () => {
    return (
        <div>
            <input />
        </div>
    )
}

```

## Interfaces

```typescript
// Types of Declarations in a TS interface

interface Props {
    text: String;
    ok: Boolean;
    i: Number; // An integer or a double
    fn: (x: String) => void; // A function returning nothing
    fn2: (j: Number) => String; // A function returning String
    obj: {  
        f1: String
    }; // An Object
}
```

- An interface accepting a JS Object interface

```typescript
interface Person {
    firstName: String
    lastName: String
}

interface Props {
    person: Person;
}
```

## Hooks

- Predefining possible values for variable count.

```typescript
const [count, setCount] = useState<number | null>(5);
```

- We can even define an object as the mandatory type for a Hooks variable

```typescript
const [variable, setVariable] = useState<{text: String}>({text: 'hello'});

setVariable({text: 'new text'})
```

- We can Achieve the same thing using interfaces

```typescript
interface TextNode {
    text: String
}

const [variable, setVariable] = useState<TextNode>({text: 'hello'});

setVariable({text: 'new text'})
```

## Using Ref

```typescript
import React, { useRef } from 'react';

export const ComponentName: React.FC<Props> = () => {

    const inputRef = useRef<HTMLInputElement>(null);

    return (
        <div>
            <input ref={inputRef}/>
        </div>
    )
}
```

