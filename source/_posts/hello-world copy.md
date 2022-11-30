---
title: 箭头的缺点
---
1、函数内部没有arguments
2、不兼容call,apply,bind
3、不能作为对象的方法
4、代码难以阅读
1、arguments
函数内部没有arguments
/* 1、函数内部没有arguments */
function go(){
    console.log(arguments)
}
const fn  = ()=>{
    console.log(arguments);
}
go();
fn();
2、不兼容call,apply,bind
var name = "window";
function go() {
    console.log(this.name)
}
const fn = () => {
    console.log(this.name)
}
go.call({ name: "vue" });
fn.call({ name: "react" })//❌箭头函数不兼容call,bind,apply
3、不能作为对象的方法
/* 不能作为对象的方法 */
var obj = {
    name: "react",
    sayName: () => {
        console.log(this.name)
    }
}
obj.sayName(); //❌
4、代码可读性差
/* 代码可读性差 */
var a =20;
const fn = ()=>a>10? '正确':'错误';