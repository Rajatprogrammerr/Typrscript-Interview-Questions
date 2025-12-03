Interpreter reads code and translates it line by line into what's known as machine code or Byte code (runtime)

"Compiler" catches type-related errors before the program runs, leading to more reliable code.

In Dynamically typed language, type errors are only discovered at runtime, which can lead to unexpected crashes and bugs.

Statically typed language promotes type safety and detects errors at an early stage.

# Introduction to Typescript

## What is TypeScript

TypeScript (TS) is a superset of JavaScript developed by Microsoft.

It adds static typing ability and type checking during development.

It helps detect errors before runtime, improving code quality and maintainability.

it gives static type checking while maintaining the flexibility of Javascript

detect all the errors in compile time 

## Why use TypeScript?

 Early error detection

 Better IDE support 
 (IntelliSense, autocompletion)

 More readable, maintainable, scalable code

 Used heavily in React, Angular, Node.js, Next.js

## How it works:

We use the TypeScript compiler to convert it into Javascript before executing it.
```js
tsc index.ts
node index.js

or

tsc
node index.js

TypeScript (.ts) → Transpiler (tsc) → JavaScript (.js)
```

# Basic Types

## Primitive types

number\
string\
boolean\
null \
undefined\
void\
never

TypeScript adds type annotations using : syntax.

### Example:
```js
let username: string = "Rajat";
let age: number = 22;
let isActive: boolean = true;
let hobbies: string[] = ["coding", "music"];
let marks: number[] = [90, 80, 70];
```

# Type Inference

TypeScript automatically infers the type if not specified.
```js
let city = "Delhi"; // inferred as string
city = 123; // ❌ Error
```
## null and undefined

let x = null/undefined converted into let x : any

```js
let x = null
x= 32
x= 'rajat'

let y = undefined
y= 32
y="rajat"

let z: number | undefined = undefined
z= 2
```

## The any Type

we use "any type" typically when we are in a very complex situation and we are not able to predict what the type of the variable gonna be
```js
let data: any = "Hello";
data = 100; // valid but not recommended
data.length
data()
//we can do anything without any error


function process(input:any):void{
  console.log(input)
}

process("hello")
process(3)
```

## The unknown Type
the "unknown type" is a type-safe counterpart to the "any type"

"Unknown type" provides a powerful way to handle values of certain types while maintaining type safety
```js
let input: unknown = "Hello"; 
//it forces to check the type of variable before doing anything

input + "rajat" //error
if (typeof input === "string") {
  console.log(input.toUpperCase());
}


function process(input:any):void{
  if(typeof input === "string"){

  console.log("print string", input)
  }else if(typeof input === 'number'){
    console.log("print number", input)
  }else{
    console.log("unsupported type)
  }
}

process("hello")
process(3)
```
## Typecast

```js
let x: unknown = 1

let result = (x as number) + 1
```

# Arrays and Tuple

## Array
An "Array" is a collection of items, or data, of same or different datatypes stored in contiguous memory locations, also known as database systems

```js
var arr: number[] = [1,2,3]

var arr: string[] = ["a","b","c"]

var arr: string[][] = [['raj'],['rajat']]
```

## Tuple

Tuple is a fixed length array that has defined values for each position in the array

```js
var arr:[number, number]=[1,2]
console.log(arr[0])


var arr: [number,number][] = [[1,2],[3,4]]
```

# Enums and Literals

## Literal

Literal is a textual representation (notation) of a value as it is written in source code

```js
let direction : "north" | "south" | "east" | "west"

direction = north (any of the four values)

let response: true
response = true ( can't set any other value then 'true')

```

# Enums (Enumeration)
Enums enable developers to establish a collection of named constants(enumerators), each linked with an integer value

### Numeric enum
```js
enum Size{
  small,
  medium,
  large
}

let size:Size = Size.small/0

if(size === Size.small){

}

enum Size{
  small = 10,
  medium =33,
  large = 44
}

let size:Size = Size.small/0

if(size === Size.small){
  
}
```

### String enum

```js
enum Direction{
  up = "UP",
  down = "Down"
}

enum Description{
  smallText  = "hello world"
}

console.log(Description.smallText)
```

# Optional Chaning and Bang

"Question mark" and "Exclamation point" operators allows us to check and deal with undefined values within Typescript

```js

const arr = [{name:"tim"},{name:"hal"}]

let el = arr.pop()?.name

? checks if the value is undefined or not, if it is undefined it assings the undefined to el , if not then it proceeds further
```

Exclamation point(!) operator also known as the bang

"Exclamation point(!)" operator tells the compiler to ignore the possibility of it being undefined

it forces us to move forward

```js

const arr = [{name:"raj"}]

const el = arr.pop()!.name
```

# Basic Function Types

```js
function add(a: number, b: number): number {
  return a + b;
}
```
## Arrow Function
```js
const multiply = (x: number, y: number): number => x * y;
```
## Optional & Default Parameters
```js
function greet(name: string, age?: number) {
  console.log(`Hello ${name}`);
}

function register(name: string, country: string = "India") {
  console.log(`${name} from ${country}`);
}
```
## Return Type void and never
```js
function logMessage(msg: string): void {
  console.log(msg);
}

function throwError(msg: string): never {
  throw new Error(msg);
}
```

```js
function fullNAme(firstName:string, lastName:string, middlename?:string):string{
if(middlename){
  return firstname + " " + middleName + " " + lastName
}

return firstname + " " + lastName
}

fullName("rajat","singh")


function createFn (func:(f:string, l:string,m?:string)=>string , num:number, num2:number){
  func()
}

createFn(fullName,2,3)
```

# Overload Function
"Overload function" is a function that has different call signature and can accept different types.

```js
function checkValid(name:string):void;
function checkValid(names:string[]):void;
function checkValid(nameOrNames:unknown):number{
    if(typeof nameOrNames === 'string'){
console.log("it is string")
    }else if(Array.isArray(nameOrNames)){
        console.log("it is array")
    }

    return 0 
}

checkValid(["rajat"])
checkValid("rajat")
```

# Interfaces

Interfaces define object structures and are extendable.

Interfaces is a prommaing structure/syntax that allows the computer to enforce certain properties on an object

Interfaces allows us to interact with more complex objects and understand what properties they have

mostly used with class/objects

```js
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  salary: number;
}

const emp1: Employee = {
  name: "Rajat",
  age: 22,
  salary: 50000
};
```

```js
interface Person {
    name: string,
    age: number,
    id?: number,
    hello: () => void

}

const person:Person={
    name:"rajat",
    age:32,
    hello:function(){
        console.log("hello" + this.name)
    }
}

person.hello()

```

## Type Alias
Type aliases allow you to create custom names for complex types, making your code more readable and maintainable

typically used with non-object(functions)

we cannot implement/extends the type


```js
type coordinates = [number, number]

function getCoordinates(p1:coordinates,p2:coordinates):coordinates{
return [p[0], p2[1]]
} 

```
```js
type User = {
  name: string;
  age: number;
  email?: string; // optional
};

const user: User = {
  name: "Rajat",
  age: 22
};
```

# Classes and Abstract classes 

```js
class Car {
  brand: string;
  year: number;

  constructor(brand: string, year: number) {
    this.brand = brand;
    this.year = year;
  }

  drive(): void {
    console.log(`${this.brand} is driving.`);
  }
}

const myCar = new Car("Tesla", 2024);
myCar.drive();

```

## Access Modifiers

public — accessible anywhere (default)

private — accessible only inside the class

protected — accessible in class & subclasses

```js
class BankAccount {
  private balance: number;

  constructor(initial: number) {
    this.balance = initial;
  }

  deposit(amount: number) {
    this.balance += amount;
  }
}

```

```js
class Employee {
    private name: string;
    constructor(name: string) {
        this.name = name
    }

    result(): void {
        console.log(this.name)
    }


    getName() {
        console.log(this.name)
    }
    setName(value: string) {
        this.name = value
    }
}

const person = new Employee("rajat")

person.getName()
person.setName("tanya")
person.getName()

```

## Abstract class

An Abstract Class in TypeScript is a "blueprint" for other classes. It allows you to define a base structure where:

Some methods are fully implemented (shared logic).

Some methods are abstract (signatures only) and must be implemented by the child class.

Key Rule: You cannot create an instance of an abstract class directly (e.g., new MyAbstractClass() will fail).

Use the abstract keyword before the class and any method you want to enforce on subclasses.
```js
// 1. Define the Abstract Class
abstract class PaymentGateway {
  
  // A Concrete Method: Logic shared by all children
  // 'protected' means only this class and children can use it
  protected logTransaction(amount: number) {
    console.log(`Logging transaction of $${amount} to database...`);
  }

  // An Abstract Method: NO code here. Children MUST write this.
  abstract processPayment(amount: number): void;
}

// ------------------------------------------

// 2. Implement Child Classes
class StripePayment extends PaymentGateway {
  processPayment(amount: number): void {
    this.logTransaction(amount); // Reusing shared logic
    console.log(`Connecting to Stripe API for $${amount}`);
  }
}

class PayPalPayment extends PaymentGateway {
  processPayment(amount: number): void {
    this.logTransaction(amount);
    console.log(`Redirecting to PayPal for $${amount}`);
  }
}

// 3. Usage
const payment = new StripePayment(); // ✅ Works
// const base = new PaymentGateway(); // ❌ Error: Cannot create an instance of an abstract class
```
## class and interface

```js
interface Animal{
    speak():void
}

class Dog implements Animal{
    name:string;
    color:string;

    constructor(name:string, color:string){
        this.name = name
        this.color = color
    }

    speak(): void {
        console.log(this.name)
    }

    test(){
        console.log("true")
    }
}

const dog:Animal = new Dog("labra", "white")

dog.speak()
```

# Static variable/ methods

```js
class Dog {
    static count: number = 0

    name: string

    constructor(name: string) {
        Dog.count++
        this.name = name
    }

    static dec() {
        this.count--
    }

}

const dog = new Dog("labra")
console.log(Dog.count)
const dog2 = new Dog("labra")
console.log(Dog.count)
Dog.dec()
console.log(Dog.count)
```

# Generics
Generics make code reusable and type-safe.

It allows us to have more flexible functions, methods, classes, etc that can accept any different data types

```js
class Product<T> {
    private item: T[] = []

    addItem(value: T): void {
        this.item.push(value)
    }

    getItem(): T[] {
        return this.item
    }


}

const p = new Product()
p.addItem("rajat")
p.addItem(4)
p.addItem(3)
let data = p.getItem()
console.log(data)

```

```js
function money<k, v>(key: k, value: v) {
    if (key) {
        console.log(value)
    } else {
        console.log("invalid key")
    }
}

money<number,string>(12636734,"success")

```

# Union and Intersection

This allow us to combine multiple types together to create some more complex types

## Union

```js
type dataValue = string | number | boolean

function value(a:dataValue):dataValue{
  return a
}

const data = value("rajat")
console.log(data)

```
## Intersection

```js
type Admin = { name: string; privileges: string[] };
type Employee = { startDate: Date };

type ElevatedEmployee = Admin & Employee;

const emp: ElevatedEmployee = {
  name: "Rajat",
  privileges: ["server-access"],
  startDate: new Date()
};
```

# Type guards

Type guards are a way of checking the type of a value at runtime and narrowing its type in a conditional block

typeof
instanceof
in

## typeof

```js
type stringOrNumber = string | number

function data(value: stringOrNumber): void {
    if (typeof value === "string") {
        console.log("it is string")
    } else {
        console.log("it is number")
    }
}

data(2)
```

## Instanceof

```js
class Dog {
    firstName: string
    lastName: string

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName
        this.lastName = lastName
    }

}
class Cat {
    firstName: string


    constructor(firstName: string) {
        this.firstName = firstName

    }

}

function getName(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        console.log(animal.firstName + " " + animal.lastName)
    } else {
        console.log(animal.firstName)
    }
}

getName(new Dog("tommy", "kumar"))
getName(new Cat("tom"))

```

## in

```js
class Dog {
    firstName: string
    lastName: string

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName
        this.lastName = lastName
    }

}
class Cat {
    firstName: string


    constructor(firstName: string) {
        this.firstName = firstName

    }

}

function getName(animal: Dog | Cat) {
    if ("lastName" in animal) {
        console.log(animal.firstName + " " + animal.lastName)
    } else {
        console.log(animal.firstName)
    }
}

getName(new Dog("tommy", "kumar"))
getName(new Cat("tom"))

```

## is

```js
class Dog {
    firstName: string
    lastName: string

    constructor(firstName: string, lastName: string) {
        this.firstName = firstName
        this.lastName = lastName
    }

}
class Cat {
    firstName: string


    constructor(firstName: string) {
        this.firstName = firstName

    }

}

function isDog(pet: Dog | Cat): pet is Dog{
  return (pet as Dog).lastName !==undefined
}

function getName(animal: Dog | Cat) {
    if (isDog(animal)) {
        console.log(animal.firstName + " " + animal.lastName)
    } else {
        console.log(animal.firstName)
    }
}

getName(new Dog("tommy", "kumar"))
getName(new Cat("tom"))

```

# Discriminated Unions

```js
type Log = Warning | Info | Success

interface Warning {
    type: "warning";
    msg: string
}
interface Info {
    type: "info";
    text: string
}
interface Success {
    type: "success";
    message: string
}

function hello(log: Log) {
    switch (log.type) {
        case "warning": console.log(log.msg)
            break;

        case "info": console.log(log.text)
            break;

        case "success": console.log(log.message)
    }
}

hello({ type: "success", message: "it is success" })
hello({ type: "info", text: "it is info" })
hello({ type: "warning", msg: "it is warning" })


```

# Utility Types

TypeScript utility types are built-in types that enable you to transform and manipulate existing types in various ways

Utility Types are built-in TypeScript helpers that transform or manipulate existing types to make them more flexible or reusable — without rewriting the entire type.

They are part of the TypeScript standard library, and they work mostly with interfaces, type aliases, and object types.

| Utility Type   | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `Partial<T>`   | Makes all properties optional                                |
| `Required<T>`  | All properties required                                      |
| `Readonly<T>`  | Makes properties read-only                                   |
| `Pick<T, K>`   | Picks selected keys                                          |
| `Omit<T, K>`   | Omits selected keys                                          |
| `Record<K, T>` | Creates object type with keys of type K and values of type T |


## Partial utility types

it takes an existing type and makes all its properties optional

```js
interface Todo{
  title:string,
  day:number
}

function task(value: Partial<Todo>){
  value.title
}

```

## Readonly 

it is used to create a new type where all properties are read only, meaning they cannot be modified once assigned a value

```js
interface Todo{
  title:string
}

const myTodo : Readonly<Todo> = {titile:"heelo"}
myTodo.title = "hlo" (cannot do)
```

## Record 
```js

interface PageInfo{
  title:string
}

const pages:Record<string, PageInfo> ={
home:{title:"home"}
about:{title:"About"}
}

const pageNumber:Record<number, PageInfo> ={
0:{title:"home"}
1:{title:"About"}
}
```

## Pick

allows us to create a new types by picking a set of properties from an existing type

Selects specific properties from a type.

Useful when you only need part of a type.

```js
interface User {
  id: number;
  name: string;
  email: string;
}

type UserName = Pick<User, "name">;

const user3: UserName = {
  name: "Rajat"
};

```

## Required

Makes all optional properties required.



```js
interface User {
  id?: number;
  name?: string;
}

type FullUser = Required<User>;

const user2: FullUser = {
  id: 1,
  name: "Rajat"
}; // ✅ must provide all properties

```

## Omit<T, K>


Removes specific properties from a type.

```js
interface User {
  id: number;
  name: string;
  email: string;
}

type UserWithoutEmail = Omit<User, "email">;

const user4: UserWithoutEmail = {
  id: 1,
  name: "Rajat"
};

```

## Record<K, T>


Constructs an object type with keys of type K and values of type T.

```js
type Roles = "admin" | "user" | "guest";

const rolePermissions: Record<Roles, string[]> = {
  admin: ["create", "read", "update", "delete"],
  user: ["read"],
  guest: []
};

```
 Great for creating dictionary or map-like objects.

 ## Parameters<T>


Extracts the parameter types of a function as a tuple.

```js
function add(a: number, b: number) {
  return a + b;
}

type AddParams = Parameters<typeof add>;
// type AddParams = [number, number]

```

# Modules & Imports

```js
math.ts

export function add(a: number, b: number): number {
  return a + b;
}

//named export


function sub(a:number, b:number):number{
  return a-b
}


export default sub //default export
```
```js
main.ts

import { add } from "./math"; //named import
import anything from "./math"; //default import - we can import by any name
console.log(add(10, 5));
```
# extends vs implements
Here is the breakdown of the difference between extends and implements in TypeScript.

The simplest way to remember it:

extends = Inheritance (I am a version of that, and I get its code).

implements = Contract (I promise to have these features, but I must write the code myself).

## 1. extends (Inheritance)
Used when a Class inherits from another Class (or an Interface from an Interface).

Relationship: "Is-a" relationship (e.g., A Dog is an Animal).

What you get: You inherit behavior (code). The child class automatically gets the methods and properties of the parent.

Limitation: A class can only extend one parent class.

Example: Class Extending Class

```TypeScript
class Animal {
  move() {
    console.log("Moving...");
  }
}

// Dog inherits the 'move' method for free
class Dog extends Animal {
  bark() {
    console.log("Woof!");
  }
}

const myDog = new Dog();
myDog.move(); // ✅ Works (inherited from Animal)
myDog.bark(); // ✅ Works (defined in Dog)
```
Example: Interface Extending Interface (Common in React)
This is how you combine props in React.


```TypeScript
interface BasicProps {
  id: string;
  className?: string;
}

// UserCardProps has id, className, AND username
interface UserCardProps extends BasicProps {
  username: string;
}
```
## 2. implements (Contract)
Used when a Class agrees to satisfy an Interface.

Relationship: "Acts-like" relationship.

What you get: You get NO code. You only get Rules. If the interface says you need a save() method, you must write a save() method in the class, or TypeScript will throw an error.

Superpower: A class can implement multiple interfaces.

Example: Class Implementing Interface

```TypeScript
interface ILogger {
  log(message: string): void;
}

interface IDatabase {
  save(data: any): void;
}

// This class promises to follow the rules of BOTH interfaces
class UserService implements ILogger, IDatabase {
  
  // ❌ If I delete this method, TypeScript yells at me
  log(message: string) {
    console.log(message);
  }

  // ❌ If I delete this, TypeScript yells
  save(data: any) {
    console.log("Saving data...", data);
  }
}
```

# Void vs never

The main difference lies in whether the function finishes executing.

void: The function finishes running, but it doesn't return a value (technically returns undefined).

never: The function never finishes running (it crashes, throws an error, or loops forever).

## 1. void (Nothing is returned)
Use void when a function performs a "side effect" (like logging to the console, saving to a database, or updating state) and then completes successfully.

Does the code continue? Yes.

What is the value? Technically undefined.


```TypeScript
function logMessage(msg: string): void {
  console.log(msg); 
  // The function reaches this line and finishes naturally.
}

// Usage
logMessage("Hello");
console.log("This line WILL run"); // ✅ Code continues
```
Common Next.js Use Case: Event Handlers and useEffect.

```TypeScript

// Button click returns nothing, just triggers an action
const handleClick = (): void => {
  setCount(count + 1);
};
```
## 2. never (The end is unreachable)
Use never when the function is not supposed to finish. If a function has a return type of never, it means the program will stop or hang inside that function.

Does the code continue? No.

What is the value? None (not even undefined).

Scenario A: Throwing Errors
```TypeScript

function throwError(msg: string): never {
  throw new Error(msg); 
  // The function CRASHES here. It never reaches the closing brace '}'.
}

// Usage
throwError("Something broke");
console.log("This line will NEVER run"); // ❌ Unreachable code
Scenario B: Infinite Loops
TypeScript

function keepProcessing(): never {
  while (true) {
    console.log("I am stuck here forever...");
  }
  // Unreachable end of function
}
```
# explain optional chaining and bang in typescript
Here is the explanation of Optional Chaining (?.) and the Bang Operator (!) in TypeScript.

They are opposites:

?. (Optional Chaining): "I'm not sure if this exists. If it doesn't, just stop and return undefined." (Safe).

! (Bang / Non-Null Assertion): "I know for a fact this exists. Stop checking and trust me." (Unsafe/Strict).

## 1. Optional Chaining (?.)
This is a safety mechanism. It stops your app from crashing with the error "Cannot read properties of undefined".

If the part before the ?. is null or undefined, the expression automatically short-circuits and returns undefined.

Syntax & Usage
```TypeScript

interface User {
  profile?: {
    name: string;
    details?: {
      age: number;
    }
  }
}

const user: User = {}; // Empty object

// ❌ OLD WAY (Verbose)
// If we access user.profile.name, it crashes because profile is undefined
const name1 = user.profile && user.profile.name; 

// ✅ NEW WAY (Clean)
// If profile is missing, name2 becomes 'undefined'. No crash.
const name2 = user.profile?.name; 

// Deep nesting works too
const age = user.profile?.details?.age;
```
## In React/Next.js
You will use this constantly when rendering data from an API, because data is usually null before the fetch completes.

```TypeScript

export default function UserCard({ data }: { data: any }) {
  return (
    <div>
      {/* Without ?., this line would crash the app while loading */}
      <h1>{data?.user?.name}</h1>
      
      {/* Works with arrays too */}
      <p>First Tag: {data?.tags?.[0]}</p>
    </div>
  );
}
```
## 2. The Bang Operator (!)
This is the Non-Null Assertion Operator. You use it to tell TypeScript: "I guarantee this value is not null/undefined, so don't throw an error."

It does not change the runtime code. It only silences the TypeScript compiler.


```TypeScript
const myString: string | null = "Hello";

// TypeScript Error: Object is possibly 'null'.
const len1 = myString.length; 

// ✅ Fix with Bang
// We are forcing TS to treat it as a string.
const len2 = myString!.length; 
```
# When to use it?
## 1. Environment Variables: Next.js treats all env variables as string | undefined. If you are sure it's defined in your .env file, use !.

```TypeScript

// lib/db.ts
// TS thinks this might be undefined, but we know it's there.
const dbUrl = process.env.DATABASE_URL!; 
```
## 2. React Refs: When you know the DOM has loaded and the ref is attached.

```TypeScript
const inputRef = useRef<HTMLInputElement>(null);

function focusInput() {
  // We know 'current' exists because we called this on a button click
  inputRef.current!.focus();
}
```
## The Danger Zone 
If you use ! but the value actually is null at runtime, your app will crash.


```TypeScript
function dangerous(user: any) {
  // If user is null, this crashes with:
  // "Cannot read properties of null (reading 'name')"
  console.log(user!.name); 
}
```