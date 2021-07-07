console.log("Hello from typescript");
/* TypeScript - Makes JS as statically typed language*/
// function setUserOnline(userId: number, onlineStatus: string) {
// 	//Business logic
// 	console.log(typeof userId);
// }
// setUserOnline("1234", "online");
// setUserOnline(1234, "offline");
// function add(number1: number, number2: number) {
// 	console.log(number1 + number2);
// }
// add(2, 4);
// add("2", "4");
/**
 * string
 * number
 * boolean
 * null
 * undefined
 */
//Type Inferenece
// let age: number = 40;
// let userName = "usr@wer";
// userName = false;
// var isUserOnline1 = false;
// isUserOnline1 = "false";
// let isUserOnline: boolean = false;
// const price = 12.4;
// let productPrice = 24;
// productPrice = 20;
// const userName = "userone";
/*
const userName;
userName = "userone"
*/
// const userInfo = {
// 	name: "userone",
// 	id: "12"
// };
/**
 * const userInfo;
 * value will be stored in heap
 * unserInfo -> head/Memory address
 */
// const users = ["userone", "usertwo"];
// users.push("userthree");
// userInfo.name = "usertwo";
//Arrays - Dynamic length and dynamic types for each values
// const users = ["username", "username1", 2, 4, 5];
// const strUser: string[] = ["test", 1, false];
// const numbers: number[] = [1, 2, 4, false, "asdasd"];
var usersInfo = [
    {
        name: "some Name",
        location: "some loation",
        age: 20,
        id: "123",
        email: "username@email.com"
    },
    {
        name: 40,
        location: "some loation",
        age: 20,
        email: null
    }
];
var userInfo = {
    name: "some Name",
    loaction: "some loation",
    age: 20,
    isOnline: false,
    email: "user@email.com"
};
var userInfo2 = {
    name: "some Name",
    loaction: "some loation",
    age: 20,
    isOnline: false
};
// console.log(userInfo.email);
// console.log(userInfo2.email);
// const newPerson: Person = userInfo;
// <div>{{newPerson.age}}</div>
// newPerson.name = "api name";
// newPerson.location = "banglore";
// newPerson.age = 20;
// const person: {
// 	name: string;
// 	location: string;
// 	age: number;
// } = {
// 	name: "Test",
// 	location: "Banglore",
// 	age: 12
// };
// let userName: string | boolean | number = "false";
// userName = false;
// userName = 12;
/**
 * class in Typescript
 * Access modifiers for the properties in class
 */
var User = /** @class */ (function () {
    function User(userName, id, phone) {
        this.name = userName;
        this.id = id;
        this.phone = phone;
    }
    User.prototype.getcombineduserdetails = function () {
        return this.name + this.id;
    };
    return User;
}());
var user1 = new User("user1", "user_1", 123456789);
var user2 = new User("user2", "user_2", 123456789);
// console.log(user1.id);
// user1.id = "asdsad";
// console.log(user1);
// console.log(user2.getcombineduserdetails());
/**
 * Function and its return types
 */
// function add(number1: number, number2: number) {
// 	return number1 + number2;
// }
// function sayHello(): string {
// 	console.log("hey! Hello");
// 	return "test";
// }
// sayHello();
// function throwError(message: string, code: number) {
// 	throw { message: message, error_code: code };
// }
// let functionProp: (number1: number, number2: number) => number;
// functionProp = add;
// functionProp = sayHello;
// functionProp();
/**
 * Closure in Javascript
 */
//global scope / local scope
//Global Scope
// var userName = "userName";
// function printUserName() {
// 	//Function/local scope
// 	var userId = "123";
// 	// var userName = "test";
// 	console.log(userName);
// 	console.log(userId);
// }
// printUserName();
/**
 * sum(1,2) - 3
 * sum(1)(2) - 3
 */
// function sum(a) {
// 	a = 1;
// 	return function (b) {
// 		//Will have access to parent scope
// 		return a + b;
// 	};
// }
// console.log(sum(1, 2));
// console.log(sum(1)(2));
/**
 * sum(1)(2)
 * sum(1) => a =1; return function (b) {
        return a + b;
    };
  sum(1)(2) = return 1+2 => 3
 * */
var obj = {
    name: "username",
    printName: function () {
        console.log(this.name, "---------");
    }
};
obj.printName();
// Object.freeze(obj);
// obj.age = 23;
