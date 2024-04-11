# Generic Types

`Generics` are flexible and general types that can be converted into a type each time according to our needs.

```tsx
function placeHolder <T> ( args : T[]): T[] {
	return args;
}

let numPlaceHolder = placeHolder<number>([100, 200, 300]); // ✅
let strPlaceHolder = placeHolder<string>(["100", "200", "300"]); // ✅

function multiGeneric<T, U>(id:T, name:U): U {
	return name;
}

multiGeneric<number, string>(1, "Quera");
```

---

## **Utility Types:**

- **Partial<Type>**
    
    `Partial` makes all properties of a type optional.
    
    ```tsx
    type User = {
      id: number;
      name: string;
    };
    // Partial<User> === {
    //   id?: number;
    //   name?: string;
    // };
    
    let firstUser: User = {
      id: 1,
      name: "Quera",
    }; // ✅
    
    let firstUser: User = {
      name: "Quera",
    }; // ❌ Property 'id' is missing in type '{ name: string; }' but required in type 'User'.
    
    let partialUser : Partial<User>  = {
      name: "Quera",
    }; // ✅
    ```
    
- **Required<Type>**
    
    `Required` makes all properties of a type mandatory all values.
    
    ```tsx
    type User = {
      id?: number;
      name?: string;
    };
    // Required<User> === {
    //   id?: number;
    //   name?: string;
    // };
    
    let firstUser: User = {
      name: "Quera",
    }; // ✅
    
    let secondUser : Required<User> = {
      name: "Quera",
    }; // ❌ Property 'id' is missing in type '{ name: string; }' but required in type 'Required<User>'.
    
    let requiredUser : Required<User> = {
      id: 1,
      name: "Quera",
    }; // ✅
    ```
    
- **Readonly<Type>**
    
    `Readonly` makes all properties of a type all values read-only.
    
    ```tsx
    type User = {
      id: number;
      name: string;
    };
    // Readonly<User> === {
    //   readonly id: number;
    //   readonly name: string;
    // };
    
    let firstUser : Readonly<User> = {
      id: 1,
      name: "Quera",
    };
    
    firstUser.name = "college"; // ❌ Cannot assign to 'name' because it is a read-only property.
    ```
    
- **Record<Keys, Type>**
    
    `Record` combines two types. `Record` receives two parameters, the first parameter specifies the type of `keys` and the second parameter specifies the type of `values`.
    
    ```tsx
    interface PageInfo {
    title: string;
    }
    type Page = "home" | "about" | "contact";
    // Record<Page, PageInfo> === {
    //	 about: PageInfo;
    // 	 contact: PageInfo;
    // 	 home: PageInfo
    // }
    
    const nav : Record<Page, PageInfo> = {
    	about: { title: "about" },
    	contact: { title: "contact" },
    	home: { title: "home" },
    }; // ✅
    ```
    
- **Pick<Type, Keys>**
    
    `Pick` receives two parameters, the first is the type and the second is the `union type` or the fields of the first type that we want to be in the new type that we created.
    
    ```tsx
    type User = {
      id: number;
      name: string;
      age: number;
    };
    type UserName = Pick<User, "id" | "name">;
    // Pick<User, "id" | "name"> === {
    //	 id: number;
    //	 name: string;
    // }
    
    let pickUserName : UserName = {
      id: 1,
      name: "Quera",
    }; // ✅
    
    let secondUserName : UserName = {
      id: 1,
      name: "Quera",
      age: 20,
    }; // ❌ Type '{ id: number; name: string; age: number; }' is not assignable to type 'UserName'.
    //Object literal may only specify known properties, and 'age' does not exist in type 'UserName'.
    ```
    
- **Omit<Type, Keys>**
    
    `Omit` works similarly to `Pick`, that is, it creates a new custom type based on the existing types, except that the second parameter is the fields we want the new type to be absent from.
    
    ```tsx
    type User = {
      id: number;
      name: string;
      age: number;
      lastActive: number;
    };
    type UserName = Omit<User, "id" | "name">;
    // Omit<User, "id" | "name"> === {
    //	 age: number;
    //	 lastActive: number;
    // }
    
    let omitUserName : UserName = {
      age: 20,
      lastActive: 16302124725,
    }; // ✅
    
    let secondUserName : UserName = {
      name: "Quera",
      age: 20,
      lastActive: 16302124725,
    }; // ❌ Type '{ name: string; age: number; lastActive: number; }' is not assignable to type 'UserName'.
    //Object literal may only specify known properties, and 'name' does not exist in type 'UserName'.
    ```
    
- **Exclude<UnionType, ExcludedMembers>**
    
    `Exclude` has two input parameters, the first parameter is `Union Type` and the second parameter is `Excluded Members`. `Exclude` removes the values of the second parameter that are the same as the first parameter.
    
    ```tsx
    type UnionType = "val1" | "val2" | "val3" | "val3"
    // Exclude<UnionType, "val1" | "val2"> === "val3" | "val3"
    
    let noVal1Or2: Exclude<UnionType, "val1" | "val2"> = "val3"; // ✅
    
    let noVal3: Exclude<UnionType, "val3"> = "val3" // ❌ Type '"val3"' is not assignable to type '"val1" | "val2" | "val4"'.
    ```
    
- **Extract<Type, Union>**
    
    `Extract` takes the desired type as the first parameter and receives the type we want to extract from the first type as the second parameter.
    
    ```tsx
    type UnionType = "a" | "b" | "c";
    type ExtractType = Extract<UnionType, "a" | "f">;
    // Extract<UnionType, "a" | "f"> === "a"
    
    let extractVariable : ExtractType = "a"; // ✅
    
    extractVariable = "c"; // ❌ Type '"c"' is not assignable to type '"a"'.
    
    extractVariable = "f"; // ❌ Type '"f"' is not assignable to type '"a"'.
    ```
    
- **NonNullable<Type>**
    
    We use `NonNullable` to remove `null` and `undefined` values.
    
    ```tsx
    type NonnullableType = string | number | null | undefined;
    type NoNulls = NonNullable<NonnullableType>;
    // NonNullable<NonnullableType> === string | number
    
    let noNullsVariable : NoNulls = "Quera"; // ✅
    
    noNullsVariable = null; // ❌ Type 'null' is not assignable to type 'NoNulls'.
    ```
    
- **Parameters<Type>**
    
    We use `Parameters` to get the parameter type of a function that takes a function as input and returns a `tuple`.
    
    ```tsx
    function myFunc(a: string, b: number, c: boolean) {
    	// ...
    }
    
    type FuncParams = Parameters<typeof myFunc>;
    // Parameters<typeof myFunc> === [string, number, boolean]
    
    let params: FuncParams = ['sina', 30, true]
    ```
    

- **ReturnType<Type>**
    
    `ReturnType` ****is used to create a type of the output type of a function
    
    ```tsx
    const func1 = (): number => {
    	// ...
    };
    const func2 = (): [number, number] => {
    	return [2, 3]
    };
    
    type T1 = ReturnType<typeof func1>;
    // ReturnType<typeof func1> === number
    
    type T2 = ReturnType<typeof func2>;
    // ReturnType<typeof func2> === [number, number]
    ```
    

- **Awaited<Type>**
    
    If the output type is a `Promise`, you can use `Awaited` to find the `Promise` type, and you can also get the type of nested promises.
    
    ```tsx
    type A = Awaited<Promise<string>‌>;
    // Awaited<Promise<string>‌> === string
               
    type B = Awaited<Promise<Promise<number>‌>‌>;
    // Awaited<Promise<Promise<number>‌>‌> === number
    
    type C = Awaited<boolean | Promise<number>‌>;
    // Awaited<boolean | Promise<number>‌> ===  number | boolean
    ```
    
- **ReadonlyArray<Type>**
    
    We use `ReadonlyArray` to have a `Readonly` array.
    
    ```tsx
    const numbers: ReadonlyArray<Number> = [1, 2, 3];
    
    numbers.push(4); // ❌ Error: Property 'push' does not exist on type 'readonly Number[]'
    ```
    
- **ConstructorParameters<Type>**
    
    `ConstructorParameters` is used when we want to have a type of `constructor` parameters of a class that will eventually return a `tuple`.
    
    ```tsx
    class MyClass {
      constructor(id: string, age: number) {
    		// ...
      }
    }
    
    const x: ConstructorParameters<typeof MyClass> = ["id1234", 30];
    // ConstructorParameters<typeof MyClass> === [string, number]
    ```
    

see all of utility types see [all-utility-types](https://github.com/sinasedaghat/typescript-hints/blob/main/files/all-utility-types.ts) file.

---

## **Generics Concepts:**

- **Generic Functions**
    
    ```tsx
    function identity<Type>(arg: Type) {
        return arg;
    }
    
    console.log(identity(1));
    
    console.log(identity('1'));
    ```
    

- **Generic Interfaces**
    
    ```tsx
    interface KeyBoard  <T, U> {
      key: T;
      value: U;
    }
    
    const button1: KeyBoard<string, string> = { key: "a", value: "01100001" };
    
    const button2: KeyBoard<number, string> = { key: 1, value: "00000001" };
    
    const button3: KeyBoard<boolean, number> = { key: false, value: 0 };
    ```
    
- **Generic Classes**
    
    ```tsx
    class Person<T, U, C> {
      name: T
      age: U
      hobby: C[]
    
      constructor(name: T, age: U, hobby: C[]) {
        this.name = name
        this.age = age
        this.hobby = hobby
      }
    }
    
    const output = new Person<string, number, string>('john doe', 23, ['swimming', 'traveling', 'badminton'])
    
    console.log(output)
    ```
    

- **Extending Generic Classes**
    
    We have received the input generic type `T` from `CompressibleStore` and passed it to `Store`.
    
    ```tsx
    class Store<T> {
      protected _objects: T[] = [];
      
      add(obj: T): void {
        this._objects.push(obj);
      }
    }
    
    class CompressibleStore<T> extends Store<T> {
      compress() {
        // compressing process...
      }
    }
    ```
    
    By `extend` the input of the generic type `T` from the `{name: string}` object to Typescript, we understand that we expect any input type that is passed as generic `T`, must be of object type and must have a property with `name` key and `string` type.
    
    ```tsx
    class Store<T> {
      protected _objects: T[] = [];
      
      add(obj: T): void {
        this._objects.push(obj);
      }
    }
    
    class SearchableStore <T> extends Store<T>{
      find(name: string): T | undefined {
        return this._objects.find( (obj) => obj.name === name); // ❌ Property 'name' dose not exist on type 'T'.
      }
    }
    
    class SearchableStore<T extends { name: string }> extends Store<T> {
      find(name: string): T | undefined {
        return this._objects.find((obj) => obj.name === name);
      }
    }
    ```
    
    ```tsx
    interface Product {
      //...
    }
    
    class ProductStore extends Store<Product> {
      filterByCategory(category: string): Product[] {
        // filtering process...
        return [];
      }
    }
    ```
    
- **Generic Constraints**
    
    This restriction is the same as inheritance and makes all types in `Employees` mandatory for `Secretary`, and `Secretary` must include all values in `Employees`.
    We can add any additional values to `Secretary` that are not in `Employees`, but all the values of `Employees` must be present in it. `Secretary` can be said to contain "all the values of `Employees` plus any additional values".
    
    ```tsx
    interface Employees {
      name: string;
      age: number;
      jobID: string;
    }
    interface  Secretary{
      name: string;
      age: number;
      jobID: string;
      address: string;
      employeeID: number;
      education: string;
    }
    
    function handleInformations <T1, T2 extends T1>(suorce: T1): T2 {
      return Object.apply({}, suorce);
    }
    
    const firstEmployee: Employees = {
      name: "sahar",
      age: 60,
      jobID: "secretary0012",
    };
    
    const employeeInformation = handleInformations <Employees, Secretary> (
      firstEmployee
    );
    
    interface  Secretary{
      name: string;
      jobID: string;
      address: string;
      employeeID: number;
      education: string;
    } // ❌ Type 'Secretary' does not satisfy the constraint 'Employees'. Property 'age' is missing in type 'Secretary' but required in type 'Employees'.
    ```
    
- k**eyof operator**
    
    The `keyof` operator takes a typed object and creates a string or `union` of its keys.
    
    ```tsx
    type Point = { x: number; y: number };
    type P = keyof Point; // === "x" | "y"
    
    type Arrayish = { [n: number]: unknown };
    type A = keyof Arrayish; // === number
    
    type Mapish = { [k: string]: boolean };
    type M = keyof Mapish; // === number | string
    ```
    
- **Mapped Types**
    
    `Pick` utility type receives two generic types `T` and `K` as input; So that `K` can only be a set of keys of `T`. (`K extends key of T`)
    Finally, we have determined that for each key in `K`, we will have a key with the same type as in `T`. (`[P in K]: T[P];`)
    
    ```tsx
    type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
    
    type Pick<T, K extends keyof T> = {
      [P in K]: T[P];
    };
    ```
    
    By placing the `-` sign before the `readonly` keyword, it removes the `readonly` feature and makes the desired properties changeable.
    By placing the sign `-` before the `?` (question mark) removes the optional feature of the desired properties and in other words makes it mandatory.
    
    ```tsx
    type CreateMutableAndRequired<Type> = {
      -readonly [Property in keyof Type]-?: Type[Property];
    };
    
    type LockedAccount = {
      readonly id: string;
      readonly name: string;
    };
    
    type UnlockedAccount = CreateMutableAndRequired<LockedAccount>;
    ```