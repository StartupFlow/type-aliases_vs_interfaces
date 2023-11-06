### some reasons to use type alias over interface

 ![from typescript docs](type-interface.png)

 #### let's start with the similarities:
 ```ts
// we can use both to describe an object
 type UserProps = {
      name: string;
      age: number;
      address: string;
 }

  interface UserProps {
        name: string;
        age: number;
        address: string;
  }

 // we use Intersections to combine types whereas we use extends to combine interfaces

  type UserProps = {
        name: string;
        age: number;
  }
  type AdminProps =UserProps & {
        role: string;
  }

  // we "extend" interfaces
  interface UserProps {
        name: string;
        age: number;
  }

  interface AdminProps extends UserProps {
        role: string;
  }


 ```


1. *interface* can only describe **object** type whereas *type aliases* can describe **object** AND **primitive** types (e.g. string, number, boolean, etc.)

   ```ts
   type AddressProps = string;
   const displayAddress = (address: Address) => console.log(address);
   ```

    ```ts
    interface AddressProps {
        address: string;
    }
    // we must use addressObject.address to access the value
    const displayAddress = (addressObject: AddressProps) => console.log(addressObject.address);
    ```


    1. *type aliases* can describe union types whereas *interface* cannot:

      ```ts
      type AddressProps = string | string[];
      let address = "42, rue des jeûneurs"; // ✅
      address = ["42, rue des jeûneurs"]; // ✅
      ```

      ```ts
      interface StringAddressProps {
          address: string
      } | // ❌ // an interface cannot describe union types since we don't have an equal sign and we always describe a single object
      interface StringArrayAddressProps {
          address: string[]
      }
      ```
1. *type aliases* can easily use **utility types** - interface can too but with an ugly syntax

      ```ts
      type UserProps = {
          name: string;
          age: number;
          createdAt: Date;
      }

      type GuestProps = Omit<UserProps, "name" | "age"> // ✅

      ```

      ```ts
      interface UserProps {
          name: string;
          age: number;
          createdAt: Date;
      }
      interface GuestProps extends Omit<UserProps, "name" | "age"> {} // we must add an empty curly braces to make it work
      ```
      1. *type aliases* can easily describe **tuples** - interface can too but with an ugly syntax

      ```ts
      type Address = [number, string];

      const address: Address = [28, "rue des jeûneurs"]; // ✅
      ```

      ```ts
  interface Address extends Array<number | string> {
      0: number;
      1: string;
  }
      ```

1. extracting type from something else
  ```ts
  const company = {
      name: "Apple",
      address: "1 Infinite Loop",
      employees: 100000,
    tagTree = {
      name: "Tech",
      tags: ["Hardware", "Software", "AI"],
    }
  }

  type TagTreeProps = typeof company.tagTree; // ✅
  interface TagTreeProps extends typeof company.tagTree {} // ❌ interfaces cannot extend a type directly, they can only extend other interfaces or classes.
  ```

1. *interfaces* can be **merged**. *Interfaces* are **open** and *type aliases* are **closed**

```ts
interface User {
    name: string;
}

interface User {
    age: number;
}

interface User {
    address: string;
}

const user: User = {
    name: "John",
    age: 30,
    address: "42, rue des jeûneurs",
}; // ✅ you can have multiple declarations with the same identifier (confusing, right ?). The compiler will merge all declarations of User into one single interface with all properties merged.

```
1. *type aliases* can be used for classes too

  ```ts
  type UserProps = {
      name: string;
      age: number;
  }

  class User implements UserProps {
      constructor(name: string, age: number) {
      }
  }
  ```
