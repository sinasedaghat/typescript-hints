# Object oriented and its concepts in TypeScript (classes)

## **Export and Import Modules:**

- **Named Export**
    - first type
        
        export
        
        ```tsx
        export let variable_name = 1;
        
        export function function_name() {
        	// ...
        }
        
        export class Class_Name{
        	// ...
        }
        ```
        
        ```tsx
        let variable_name = 2
        
        function function_name() {
        	// ...
        }
        
        class Class_Name {
        	// ...
        }
        
        export { variable_name, function_name, Class_Name }
        ```
        
        import
        
        ```tsx
        import { variable_name, function_name } from "./namedExport";
        
        console.log(variable_name)
        function_name()
        ```
        
        ```tsx
        import { variable_name: v_name, function_name: f_name } from "./namedExport";
        
        console.log(v_name)
        f_name()
        ```
        
    - second type
        
        export
        
        ```tsx
        export let firstShape = 100;
        
        export function secondShape() {
        	// ...
        }
        
        export class thirdShape {
        	// ...
        }
        ```
        
        import
        
        ```tsx
        import * as shapes from "./namedExport"
        ```
        
    - third type
        
        export
        
        ```tsx
        const message = () => {
          return 'Salam Donya'
        };
        
        export { message as myMessage };
        ```
        
        import
        
        ```tsx
        import { myMessage } from "./export"
        
        console.log(myMessage)
        ```
        
    
- **Default Export**
    
    export
    
    ```tsx
    const message = () => {
      const name = "Jesse";
      const age = 40;
      return name + ' is ' + age + 'years old.';
    };
    
    export default message;
    ```
    
    import
    
    ```tsx
    import message from "./defaultExport";
    
    console.log(message)
    ```
    
    ```tsx
    import { myMessage as msg } from "./export"
    
    console.log(msg)
    ```
    

---

## **Classes and their concepts:**

- **Modifiers, Getter, Setter**
    
    `readonly` and `?` In classes, they are known as `modifiers`, which make properties readonly and optional, respectively.
    In Typescript language, we can access the properties and methods of a class in three ways (`access modifiers`).
    In `public` access, we can use properties and methods defined within a class, in other classes and instances created from that class.
    In `private` access, we can only use the methods and properties inside a class, inside the same class, and we are not allowed to access them outside the class, and we put `_` before the variable names.
    `protected` access is like `private` access, but it has one more feature, which is that we can use the properties and methods of a class within the classes that inherit from that class.
    We use the `getter` method to read a property that is generally `private` within a class. We put the `get` keyword, which is specific to the `getter` method, before the desired method.
    We use the `setter` method to change a property that is generally `private` within a class. We put the keyword `set`, which is specific to the `setter` method, before the desired method.
    In classes, properties and methods are called `static`, so that we can directly access the properties and methods without creating an instance of that class.
    
    ```tsx
    class Person {
    	readonly national_no: number; // read-only property
    	public f_name: string; // public property
    	l_name: string; // public property
    	private _nickname: string // private property
    	age?: number; // optional property
    	protected phone: number; // protected property
    	static class_id_number: number = 10 // static property
    	
    	constructor(
    		f_name: string, 
    		l_name: string, 
    		nickname: string, 
    		phone: number, 
    		age?: number
    	) {
    		this.f_name = f_name;
    		this.l_name = l_name;
    		this._nickname = nickname;
    		this.phone = phone;
    		this.age = age ?? null;
    	}
    	
    	get nickName(): string { // getter method
    		return this._nickname;
    	}
    	set nickName(name: string) { // setter method
    		this._nickname = name;
    	}
    	
    	full_name(): string { // public method
    		return `${this.f_name} ${this.l_name} (${this._nickname})`	
    	}
    	
    	private showingName(): string { // private method
    		return `${this._nickname} ==> ${this.f_name}_${this.l_name}`
    	}
    	
    	static message(): string { // static method
    		return 'This is example for use class and its features'
    	}
    }
    
    class Men extends Person {
    	showName() { // public method
    		this.showingName() // ❌ method 'showingName' is private and only accessible within class 'Person'
    	}
    	
    	showPhone(): string { // public method
    		return this.phone
    	}
    }
    
    const sina = new Person('sina', 'seda', 'seda', 09123456789) // instance
    
    sina.l_name = 'sedaghat'
    console.log(sina._nickname); // ❌ Property '_nickname' is private and only accessible within class 'Person'
    console.log(sina.showingName()); // ❌ method 'showingName' is private and only accessible within class 'Person'
    console.log(sina.phone); // ❌ Property 'phone' is protected and only accessible within class 'Person' and its subclasses.
    console.log(sina.nickName);
    sina.nickName = 'SinaSeda';
    console.log(Person.class_id_number);
    console.log(Person.message());
    ```
    
- **Inheritance**
    
    Each of the inheriting classes (child), in addition to the inherited properties and methods, can also have their own properties and methods.
    To rewrite a method, it is enough to define a method with the same name after defining the child class and write the desired body inside it. It is recommended to write the `override` keyword before the method name.
    Sometimes we need to change the behavior of a method, but not in such a way that we completely rewrite it. Using the `super` keyword, we access the `define_score` method of the parent class. First, we perform the `define_score` operation as we did in the parent class (`super.define_score(score)`). Then we increase the `score` for the student.
    
    ```tsx
    class Person { // Parent class
    	score: number = 0 
    	constructor(public name: string, public age: number) {}
    
      get detail() {
        return `${this.name} ${this.age}`;
      }
    
      protected talk() {
        console.log("i am walking");
      }
      
      message() {
    	  return 'this is one person'
      }
      
      define_score(score: number) {
    	  this.score = score
      }
    }
    
    class Student extends Person { // Child class
    	gender: 'male' | 'female' | 'other'
    	
      constructor(
    	  public studentId: number, 
    	  name: string, 
    	  age: number,
    	  gender: 'male' | 'female' | 'other'
      ) {
        super(name, age)
        this.gender = gender
      }
    
      takeExam() {
    	  this.talk()
        console.log("taking exam");
      }
      
      override message() { // This method overrided in child class
    		return 'this is one student person'
      }
      
      override define_score(score: number) { // Extension of methods
    		super.define_score(score)
    		this.score += 1
      }
    }
    
    let studentOne = new Student(1, "Sina", 30, 'male')
    
    console.log(studentOne.name)
    console.log(studentOne.detail)
    console.log(studentOne.takeExam())
    ```
    
- **Abstract Classes & methods**
    
    Abstract classes are incomplete or simple classes that are never directly instantiated.
    If we do not rewrite an Abstract method (which can only exist in the Abstract Class) in the child class, we will receive an error.
    
    ```tsx
    abstract class Shape {
      constructor(public name: string) {
        this.name = name;
      }
    
      message() {
        console.log("shape is a general term, so I really don't know how to draw it!");
      }
      
      abstract render(size: number): void;
    }
    
    class Circle extends Shape {
      constructor(private diameter: number, name: string){
        super(name);
      }
      
      override render(size: number): void {
        console.log('render circle');
      } // 
    }
    
    const hexagon = new Shape('hexagon'); // ❌ Error: Cannot create an instance of an abstract class.
    const circle1 = new Circle(10, 'myCircle1'); // ✅
    const circle2 = new Circle(20, 'myCircle2'); // ✅
    const circle3 = new Circle(30, 'myCircle3'); // ✅
    ```
    
- **Interface VS Abstract Class**
    
    To implement a class from the `interface`, it is enough to use `implements` instead of `extends`. Also, we no longer need to use the `override` keyword, and we will encounter an error if we use it.
    Generally, we use `interfaces` when we don't have any implementation in the parent class. ([Interface VS Abstract Class](https://dotnetpattern.com/typescript-abstract-class#:~:text=A%20TypeScript%20Abstract%20class%20is,the%20functionality%20of%20base%20class.))
    
    ```tsx
    interface Shape {
    	name: string,
    	diameter: number,
    	render(size: number):void
    }
    
    class Circle implements Shape {
    
    	constructor( public diameter: number, public name: string){
    	  this.name = name;
    	  this.diameter = diameter;
    	}
    	render(size: number): void {
    		console.log('render circle');
    	}
    }
    ```