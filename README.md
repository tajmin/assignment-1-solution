# 1. What are some differences between interfaces and types in TypeScript?

TypeScript offers two primary ways to define the shape of data: type aliases (using the type keyword) and interfaces (using the interface keyword). A frequent question arises: when should you use one over the other?

The `type` keyword in TypeScript empowers us to give a name to any existing type. Think of it as creating a shorthand or a more descriptive label.

The `Type` aliases don't introduce new types; They provide alternative, often more semantic, names for existing ones, including primitive types.

```type User = {
id: number;
name: string;
email: string;
};
```

Here `User` provides a clear label for an object structure containing id, name, and email.

An `interface` in TypeScript defines a contract that an object must adhere to. It outlines the expected properties and their types. Consider this example:

```
interface Client {
  name: string;
  address: string;
}
```

However, certain features are exclusive to one or the other.

### Handling Primitive Types

TypeScript has built-in primitive types like number, string, boolean, null, and undefined. We can easily create a type alias for a primitive type:

```
type Address = string;
```

However, `interfaces` cannot be used to alias primitive types.

# 2. What is the use of the keyof keyword in TypeScript? Provide an example.

The `keyof` operator in TypeScript takes a type and gives back a union of its property names (as strings or numbers).

```
type Staff = {
  name: string;
  role: string;
}
```

Now if we use keyof like this:

```
type StaffKeys = keyof Staff;
```

The result will be: "name" | "role"

So StaffKeys is now a type that can only be "name" or "role". It just grabs the property names from the object.
