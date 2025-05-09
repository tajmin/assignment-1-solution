# 1. What are some differences between interfaces and types in TypeScript?

TypeScript offers two primary ways to define the shape of data: type aliases (using the `type` keyword) and interfaces (using the `interface` keyword). While `interface` lets you define the shape of objects, `type` can cover a wider types of data.

Let’s imagine a person in our code. This person must have a name, and an age.

```
const person = {
  name,
  age,
};
```

This works in JavaScript, but it's not strict—you could easily add a new property like salary, and nothing would stop you.

Now imagine you want every person object across your entire project to follow a strict shape. This is where TypeScript comes in. You could use an interface to enforce the structure:

```
interface Person {
  name: string;
  age?: number;
}
```

or `type`.

```
type Person = {
  name: string;
  age?: number;
};
```

In this context, both of these accomplish the same goal. But they’re not identical—and that’s where the differences begin to show.

### 1. Handling Primitive Types

One major difference between type and interface is how they deal with primitive types like string, number, or boolean.

`type` can alias primitives - with type, you can define an alias for a primitive type:

```
type Name = string;
type Age = number;
type IsLoggedIn = boolean;

function greetUser(name: Name): void {
  console.log(`Hello, ${name}`);
}
```

Unlike `type`, you cannot use `interface` to alias primitive.

```
interface Name = string; // Error: 'interface' declarations can only describe objects
```

<br />

### 2. Union and Intersection Types

Another key difference between type and interface is how they handle union and intersection types. A union type represents a value that can be one of several types. This can only be defined using `type`.

```
type Status = "success" | "error" | "loading";

type Input = string | number;

function handleInput(input: Input) {
  // input can be a string or a number
}
```

You cannot create a union type using `interface`:

```
interface Status = "success" | "error"; // this produces error
```

An intersection type combines multiple types into one. Both type and interface support this, though the syntax differs. Using `types`:

```
type Name = { name: string };
type Age = { age: number };

type Person = Name & Age;
```

For `interfaces`, this is how it looks:

```
interface Name {
  name: string;
}

interface Age {
  age: number;
}

interface Person extends Name, Age {}
```

### 3. Declaration Merging

Declaration merging is a key difference between interface and type in TypeScript. If you declare multiple interfaces with the same name, TypeScript will merge their properties into one.

```
interface User {
  name: string;
}

interface User {
  age: number;
}

// TypeScript merges them into:
interface User {
  name: string;
  age: number;
}

const user: User = {
  name: "Alice",
  age: 30,
};
```

`type` aliases cannot be declared multiple times with the same name in the same scope.

<br/>
<br/>

# 2. What is the use of the keyof keyword in TypeScript? Provide an example.

In TypeScript, `keyof` is a type operator used with object types. It creates a union of literal types, representing the keys of a given object type. You pass it an object, and it returns a type consisting of all the keys from that type. . It’s a powerful tool that enables you to create type-safe utilities and perform dynamic type lookups.

```
type Person = {
  name: string;
  age: number;
  active: boolean;
};

type PersonKeys = keyof Person; // PersonKeys becomes = "name" | "age" | "active"

const key: PersonKeys = "name";
```

<br/>

The `keyof` operators gives a lot of flexibility in real-world projects.

- <b>Type-safe property access</b>:
  It helps prevent errors by ensuring only valid keys of an object are used when accessing properties dynamically.

- <b>Generic utility functions</b>:
  Provides you with the flexibility to write functions that can work with various object shapes, while still ensuring key correctness.

- <b>Creating reusable types</b>:
  Useful with other type operators like T[K], Record, Pick, and Omit to build custom, reusable types.

<br/>

Let’s consider this example: we have a user object with the following properties -

```
const user = {
id: 101,
username: "pope",
isLegit: false,
};
```

If you attempt to access a property that doesn't exist:

```
const userEmail = user["email"];
```

will give you compiled code, but it fails at runtime, which is the last thing you want as a developer. This is where comes the `keyof` as savior. Let’s say we define an `interface` for our user object:

```
interface User {
  id: number;
  username: string;
  email: string;
}
```

Now let's define its' shape with the help of the declared `interface`

```
const user: User = {
  id: 1,
  username: "pope",
  isLegit: false,
};
```

Let's declare a utility function that ensures only valid keys from the User interface can be accessed in order to avoid runtime mess like above:

```
function getUserProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
return obj[key];
}
```

Now, thanks to `keyof` we can safely access valid properties of the object while it will prevent the runtime madness for non-existing properties at compile time.

```
const username = getUserProperty(user, "username"); // Type: string
const legitimacy = getUserProperty(user, "isLegit"); // Type: boolean
const email = getUserProperty(user, "email"); // Error: "email" is not a key of User
```

Saves life, huh!
