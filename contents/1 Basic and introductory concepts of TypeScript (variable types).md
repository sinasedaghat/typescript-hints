# Basic and introductory concepts of TypeScript (variable types)

## **Typescript data types:**

`boolean` `number` `string` `array` `tuple` `enum` `interface` `any` `null & undefined` `never` `unknown` `void`

- **String**
    
    ```tsx
    let name: string = "sina"
    ```
    

- **Number**
    
    ```tsx
    let age: number = "30"
    ```
    

- **Boolean**
    
    ```tsx
    let male: boolean = true
    ```
    

- **Array**
    
    ```tsx
    const numbers: number[] = [1, 2, 3];
    ```
    

- **Tuple**
    
    `tuple` are similar to arrays, except that they have a fixed length and the type of items is known.
    
    ```tsx
    let firstTuple: [number, string] = [1, "hello"];
    firstTuple = [1, "hello", 2]; // ❌ error: Type '[number, string, number]' is not assignable to type '[number, string]'. Source has 3 element(s) but target allows only 2.
    firstTuple = ["hello", 1]; // ❌ error: Type 'string' is not assignable to type 'number'. error: Type 'number' is not assignable to type 'string'.
    firstTuple.push(2); // ✅
    firstTuple.push(false); // ❌ error: Argument of type 'boolean' is not assignable to parameter of type 'string | number'.
    ```
    
    optional item
    
    ```tsx
    let rgba: [number, number, number, number?];
    rgba = [255, 0, 0]; // ✅
    rgba = [255, 255, 0, 0.8]; // ✅
    ```
    
    rest item
    
    ```tsx
    let secondTuple: [boolean, ...string[], number];
    secondTuple = [true, "item1", "item2", "item3", 203]; // ✅
    secondTuple = [true, 203]; // ✅
    ```
    

- **Enum**
    
    The typescript itself performs these values for `enum` keys (`PascalCase` is usually used to define `enum` name and key.)
    
    ```tsx
    enum Language {
     English, // English = 0,
     Spanish, // Spanish = 1,
     Russian // Russian = 2
    }
    let myFirstLanguage = Language.English // 0
    let mySecondLanguage = Language[1] // Spanish
    ```
    
    In `enum`, the string can be mapped to the number. (with the `key` you have access to `value` and vice versa)
    
    ```tsx
    enum Language {
     English = 100,
     Spanish = 200 + 300,
     Russian // TypeScript infers 501 (the next number after 500)
    }
    let myFirstLanguage = Language.Russian // 501
    let mySecondLanguage = Language['English'] // 100
    ```
    
    In `enum`, the string can be mapped to the string. (with the `key`, you have access to `value`, but with `value`, you do not have access to `key`)
    
    ```tsx
    enum Color {
     Red = 'RED',
     Blue = 'BLUE',
     Pink = 'PINK',
     White = 'WHITE'
    }
    
    let myFirstColor = Color.Red // RED
    let mySecondColor = Color['Blue'] // BLUE
    let myThirdColor = Color['PINK'] // undefined
    ```
    
    When you declare an `enum` with `const`, TypeScript generates a simpler structure because it is creating a constant literal type instead of an object. (When compiled to JavaScript, this gets stripped entirely. It's important to note that using `const enum`  does not create any runtime artifact in JavaScript. It simply uses the TypeScript type system without generating any actual JavaScript code.) ([chat GPT assisted](https://chat.openai.com/share/e6ac7d52-8907-49cb-a629-bac700efbf60))
    
    ```tsx
    const enum Flippable {
     Burger = 'Burger',
     Chair = 'Chair',
     Cup = 'Cup'
    }
    ```
    

- **Object**
    
    ```tsx
    const foodObject: {name: string, price: number} = {
    	name: "kotlet",
    	price: 45000,
    }
    ```
    
    optional property
    
    ```tsx
    const foodObject: {name: string, price:number, discount:number} = {
    	name: "kotlet",
    	price: 45000,
    } // ❌ Property 'discount' is missing in type '{ name: string; price: number;}
    
    const foodObject: {name: string, price: number, discount?:number} = {
    	name:"kotlet",
    	price: 45000,
    }
    ```
    
    Some `object` may have many properties. If we want to create an `object` without having a specific list of property names, we use this method. (that is, we add as many properties as we want with different names. (`string` type))
    
    ```tsx
    const foodObject: { [index: string]: string } = {};
    
    foodObject.name= "kabab"
    foodObject.chef = "reza";
    foodObject.recipes = "meat, rice, onion";
    ```
    
    definition `object` type with `interface`.
    
    ```tsx
    interface foodObject {
    	name:string,
    	price:number,­
    }
    
    const firstFood: foodObject = {
    	name: "kotlet",
    	price: 45000,
    }
    
    const secondFood: foodObject = {
    	name: "kebab",
    	price: "45000toman", // ❌ Type 'string' is not assignable to type 'number'
    }
    ```
    

- **Interface**
    
    The name you choose for the `interface` must start with an uppercase letter.
    
    ```tsx
    interface UserInterFace {
    //...
    } // ✅
    interface userInterFace {
    //...
    } // ❌
    ```
    
    ```tsx
    interface UserInterFace{
        userName: string;
        userId:number;
    }
    
    let assignableNumber: UserInterFace = {  
     userName:123,
     userId:123
    }; // ❌ Type 'number' is not assignable to type 'string'.
    
    let missingUserId: UserInterFace = {  
     userName:"ali@gmail.com",
    }; // ❌ Property 'userId' is missing in type '{ userName: string; }' but required in type 'UserInterFace'.
    
    const addExtraProperty: UserInterFace = {
      userName: 'sina',
      userGender: 'male', // ❌ Object literal may only specify known properties, and 'userGender' does not exist in type 'UserInterFace'.
      userId: 10,
    }
    ```
    
    `interfaces` in TypeScript support declaration merging. This means that you can define an `interface` multiple times, and TypeScript will automatically merge the declarations into a single `interface`.(This can be useful when you want to extend an existing `interface` later in your code)
    
    ```tsx
    interface Person {
      name: string;
      age: number;
    }
    
    interface Person {
      gender: string;
    }
    
    const person: Person = {
      name: 'John',
      age: 25,
      gender: 'Male'
    };
    ```
    
    `interfaces` can extend other `interfaces`, allowing you to compose larger `interfaces` from smaller ones.
    
    ```tsx
    interface Shape {
      color: string;
    }
    
    interface Square extends Shape {
      sideLength: number;
    }
    
    const square: Square = {
      color: 'red',
      sideLength: 10
    };
    ```
    

- **Type Aliases**
    
    Using the keyword `type`, we have defined new types and used them further.(We start the name of `type aliases` with a capital letter T)
    
    ```tsx
    type TuserId = number | string;
    
    type Temployee = {
      userId: TuserId;
      birthDay: Date;
      salary: number;
      hiringDate: Date;
      fire: (userId:TuserId)=>Date;
    };
    ```
    
    Specified `type aliases` for function
    
    ```tsx
    type SuccessfulResponse = {
    	message: string,
      statusCode: number,
      attachment?: string, // ? The mark indicates that property is optional
    }
    
    type SuccessfulResponseHandler = (res: SuccessfulResponse) => void;
    
    const handleSuccess: SuccessfulResponseHandler = (res) => {
     // some side effects occur here
    }
    ```
    
    The types defined with the `type` keyword are also `block scope` like the variables defined with `let` and `const` and cannot be defined more than once in a `scope`.
    
    ```tsx
    type Color = 'red'
    type Color = 'blue' // ❌ Error TS2300: Duplicate identifier 'Color'.
    ```
    
    Use the `(ampersand (&)) intersection` operator to merge types. the created type has all the properties of the merge types.(the order is not important in the `intersection types`)
    
    ```tsx
    type typeCombine = typeA & typeB; // intersection types
    
    interface Person {
        name: string;
        family: string;
    }
    
    interface Contact {
        email: string;
        linkedin: string;
    }
    
    type Human = Person & Contact;
    
    let mahan: Human = {
        name: "Mahan",
        family: "Jafari",
        email: "mahan@email.com",
        linkedin: "linkedin.com/in/mahanjafarii"
    }
    ```
    

- **Union Types**
    
    By using `union type`, we can assign more than one type to a variable or parameter of a function.
    
    ```tsx
    type Length = number | string
    
    let length1: Length = '30' // ✅ 
    let length2: Length = 40 // ✅ 
    let length3: Length = true // ❌
    ```
    
    With a technique called `narrowing`, we differentiate between the input types in the function so that we can use the methods of each variable type separately for the desired type.
    
    ```tsx
    function converter(meter: number | string): number {
        if (typeof meter === 'number')
            return meter * 100;
        else
            return parseInt(meter) * 100; 
    }
    ```
    

- **Literal Types**
    
    In situations where we want to limit the value of a variable more precisely or very limited, we use the concept of `literal type`.(All data types can be defined as `literal type`)
    
    ```tsx
    type Measurement= "cm"| "inch" | "mm";
    function ruler(distance: number ,measurement:Measurement) {
        // ...
    }
    
    function ruler(distance: number ,measurement: "cm"| "inch" | "mm") {
        // ...
    }
    
    ruler(10,"cm"); // ✅ 
    ruler(1,"kg");  // ❌!
    
    let binary : 0 | 1;
    let accept : true | false;
    ```
    

- **Any**
    
    This type accepts any value and can easily store any value in it.
    We expect to receive an error but since the type of both variables we have specified is  `any`, TypeScript will not show us any error.
    
    ```tsx
    let a: any = 666 // any
    let b: any = ['danger'] // any
    let c = a + b // any // 
    ```
    

- **Unknown**
    
    The `unknown` type is very similar to `any`, it can accept any value.
    
    ```tsx
    let a: unknown = 30 // unknown
    a = '40' // unknown
    let b = a === 123 // boolean
    ```
    
    The difference between `unknown` and `any` is that we need to prove to TypeScript that we know what we're doing and are sure of its properties before accessing its value.
    
    ```tsx
    let a: unknown = 30 // unknown
    let c = a + 10 // ❌ Error TS2571: Object is of type 'unknown'
    if (typeof a === 'number') {
     let d = a + 10 // ✅ number
    } // narrowing
    ```
    

- **Void (return function)**
    
    When the function has no output, we choose `void` output type for it. (Note that only `null` and `undefined` are of `void` type.)
    
    ```tsx
    const print = (): void => {
    	console.log('print ...')
    }
    ```
    
    ```tsx
    const addTwoNumbers = (
      a: number,
      b: number,
      convertToHexadecimal?: boolean
    ): number | string => {
      const sum = a + b;
      if (convertToHexadecimal) {
        const hexString = sum.toString(16);
        return hexString;
      }
      return sum;
    }; // optional argument
    
    const addTwoNumbers = (a: number, b: number): void => {
    	console.log(a + b)
    }; // void output
    
    const addNumbers = (...numbers: number[]): number => {
    	return numbers.reduce((prev, current)=>{
    		 return prev + current;
    	}, 0 )
    }; // rest argument
    ```
    

- **Never (return function)**
    
    We have specified the output of the function as `never`, which means that we do not expect this function to return anything and have an output.
    
    ```tsx
    const never_func = (): never => {
      while(true) {
        console.log('print ...')
      }
    }
    ```
    
    The editor will easily notice that the alert() function is never executed. This is despite the fact that if we did not set the output type of greet() function to `never`, our editor would not be able to recognize this issue.
    
    ```tsx
    function greet(): never {
      while (true) {
       console.log("hello");
      }
    }
    greet();
    alert('hello') // this line is unreachable
    ```
    
    A variable with type `never` does not accept any other type.
    difference between `void` and `never` is when the output of a function is `void`, the function has an output with the value `undefined` or `null`. But in the case of `never`, the function never ends and has no output. Actually, the program stops after reaching `never`.
    
    ```tsx
    let variable: void = undefined;
    let variable1: never = null; // error ❌
    let variable2: never = 1; // error ❌
    let variable3: never = "geek"; // error ❌
    let variable4: never = true; // error ❌
    ```
    

---

## **Difference between interface and Types:**

- **Declaration merging**
    
    The TypeScript compiler automatically merges all `interface` with the same name. But if we create two `type` with the same names, but with different properties, we will encounter an error.
    
    ```tsx
    interface Movie {
    	director: string;
    };
    interface Movie {
    	name: string;
    }
    const information: Movie = {
    	director: "Tahmineh Milani",
    	name: "Pay Back"
    };
    ```
    
    ```tsx
    type Color = 'red'
    type Color = 'blue' // ❌ Error TS2300: Duplicate identifier 'Color'.
    ```
    

- **Extends and implements**
    
    Any `interface` can inherit and extend from a class. But `type` do not have this ability!
    
    ```tsx
    class CreateMovies {
     //...
    };
    
    interface Movie extends CreateMovies {
     //...
    };
    ```
    

- **Intersection**
    
    A new `type` can be made by connecting several other `type` But this feature is not true for `interface`.
    
    ```tsx
    type Teacher= {
    	name: string;
    };
    type Student= {
    	name: string;
    };
    type School= Teacher & Student;
    ```
    
    ```tsx
    interface Teacher{
    	name: string;
    };
    interface Student{
    	name: string;
    };
    interface  School Teacher & Student; // ❌ Error
    type  School= Teacher & Student;
    ```
    

- **Unions**
    
    Multiple values can be considered for `type` and this method cannot be used for `interface`.
    
    ```tsx
    type Binary = 1 | 0 ;
    
    type Movie = {
    	name: string;
    };
    type Music = {
    	name: string;
    };
    type Art = Movie | Music;
    ```
    
    ```tsx
    interface Movie {
    	name: string;
    };
    
    interface Music {
    	name: string;
    };
    interface  Art Movie | Music; // ❌ Error
    type  Art = Movie | Music
    ```
    

- **Tuples**
    
    We can declare a `type` as a `tuple`, but we cannot declare an `interface` as a `tuple`, and we can only use a `tuple` inside an `interface`.
    
    ```tsx
    type Characters = [string, number]
    
    interface Characters {
    	value: [string, number]
    }
    ```
    

---

## **Concepts related to types:**

- **Type Assertions**
    
    `Type Assertions` in TypeScript inform the compiler that a variable of one type should be processed as another type. In the above example, using the `as` keyword, we told the compiler to process the x variable as a string.
    
    ```tsx
    let x: any = "salam";
    
    let y = x as string
    
    console.log(typeof y) // output : string
    ```
    
    In the example above, we used `<>` (angle-bracket) to tell the compiler to process the variable x as a string. When we use `Type Assertions` in `TSX`, we are only allowed to use the `as` keyword.
    
    ```tsx
    
    let x: any = "salam";
    
    let y = <string> x
    
    console.log(typeof y) // output : string
    ```
    

- **Unchangeable Variables**
    
    You can add or subtract to the values inside the arrays and objects, but you cannot change their reference.
    
    ```tsx
    const cars = ["BMW"];
    cars.push("Toyota"); // ✅ cars = ["BMW","Toyota"]
    
    cars = ["Saab", "Volvo"]; // ❌ error 
    ```
    
    Using `as const` can make an array or object immutable forever.
    
    ```tsx
    const cars = ["Saab", "Volvo", "BMW"]  as const  ;
    cars.push("Toyota");  // ❌ error 
    cars = ["Toyota"]; // ❌ error 
    
    const myCars = {                                                                 
     brand: 'Toyota',                                                           
     seats: 5
    } as const  ;
    myCars.seats = 4 // ❌ Cannot assign to 'seats' because it is a read-only property.
    ```
    

- **Readonly Variables**
    
    By using `readonly` the parameters of functions or class fields will be made immutable or read-only.
    
    ```tsx
    function createCars(colors: readonly string[]) {
      colors.push('black');  // ❌ Error: Property 'push' does not exist on type 'readonly string[]'.
    }
    
    class Cars {
    	readonly name: string
      constructor(name: string) {
        this.name = name
      }
      public setName(name: string) {
        this.name = name;  // ❌ Error: Cannot assign to 'name' because it is a read-only property.
      }
    }
    ```
    

- **Optional Chaining**
    
    In TypeScript, it is possible to specify the mandatory or optional priority of types. If from the sign `?` in before `.` In fact, we gave TypeScript the option not to use it if the value of our variable or type was `null` or `undefined`, and only use it when our data had a value other than these two values.
    
    ```tsx
    type Customer = {
      birthday: Date
    };
    
    function getCustomer(id: number): Customer | null | undefined {
      return id === 0 ? null : {birthday: new Date()};
    }
    
    let customer = getCustomer(1);
    
    console.log(customer.birthday); // ❌ Error --> possibly null 
    
    console.log(customer?.birthday); // ✅ optional property access operator
    ```
    
    to mark `.?` It is also called `optional chaining`. For example, in a type, we can make the attribute inside that type optional, or if we receive an array and we don't know if the desired house in that array is empty or has a value, we can also use this operator.
    
    ```tsx
    type Customer = {
      name: string;
      birthday?: Date;
    };
    
    let customers: Array<Customer> = [{name:"masoud"},{name:"mahan"},{name:"salar"}]; // optional element access operator
    
    console.log(customers?.[0]);
    
    let logger: any = null;
    logger?.('masoud'); //optional call operator
    ```
    

- **Nullish Operator**
    
    In TypeScript and JavaScript, the values of `undefiend`, `0`, `false`, `''`, `null` have a negative charge and are actually equal to the meaning of the word `false` in booleans and are considered to be incorrect. So when we set the value of the variable to 0, TypeScript thinks that the value of our variable is not valid and assigns the default value to it instead. `Nullish` operator with `??` It is used to mean that the value of a value is valid if it is not `null` or `undefined`.
    
    ```tsx
    let speed: number | null = 0;
    
    let ride1 = {
        speed: speed || 30
    }
    
    let ride2 = {
        speed: speed ?? 30
    }
    
    console.log(ride1) // 30
    console.log(ride2) // 0
    
    speed = null
    
    console.log(ride1) // 30
    console.log(ride2) // 30
    ```
    

- **Overloading**
    
    The type of `output1` will be `Values` and the type of `output2` will be `Values` and the typescript prevents the use of methods related to strings on `output2`.
    
    ```tsx
    type Values = string | number;
    function sum(x: Values, y: Values) {
      if (typeof x === "string" || typeof y === "string") {
        return `${x} ${y}`;
      }
      return x + y;
    }
    
    const output1 = sum(100,200);
    const output2 = sum("hi","sara");
    
    output2.replaceAll('a','b'); // ❌ error
    ```
    
    The problem is solved by using the concept of `overload`.
    The work steps are as follows: we consider the two states of string and number of inputs and outputs, and add the `overload` of each of them.
    
    ```tsx
    type Values = string | number;
    function sum(x: number, y: number): number;
    function sum(x: string, y: string): string;
    function sum(x: Values, y: Values) {
      if (typeof x === "string" || typeof y === "string") {
        return `${x} ${y}`;
      }
      return x + y;
    }
    const output1=sum(100,200);
    const output2=sum("hi","sara");
    output2.replaceAll('a','b') // ✅
    ```