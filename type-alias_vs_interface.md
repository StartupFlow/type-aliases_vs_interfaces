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
