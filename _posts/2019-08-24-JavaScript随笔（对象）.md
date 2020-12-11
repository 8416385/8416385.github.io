---
layout:     post
title:      JavaScript随笔（对象）
subtitle:   JavaScript
date:       2019-08-24
author:     listen_spacer
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - JavaScript
---
#### Object构造函数或对象字面量都可以用来创建单个对象
>但有个明显的缺点：
>使用同一个接口创建很多对象，会**产生大量的重复代码**。
>为解决这个问题，人们开始使用工厂模式的一种变体。

**1、工厂模式**

>用函数来封装以特定接口创建对象的细节。
>例如：

```
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){ 
        alert (this.name);
    }
    return o;
}
var person1 = createPerson("Nicholas",29,"Software Engineer");
var person2 = createPerson("Greg",27,"Doctor");
```

>工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题
>（即怎么知道一个对象的类型）

**2、构造函数模式**

```
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    }
}

var person1 = new Person("Nicholas",29,"Software Engineer");
var person2 = new Person("Greg",27,"Doctor");
```

**构造函数与工厂模式的不同之处**
>1、没有显式地创建对象
>2、直接将属性和方法赋给了this对象
>3、没有return语句

>此外，按照惯例构造函数始终都应该以一个大写字母开头，
>而非构造函数则应该以一个小写字母开头。
>要创建Person的新实例，必须使用new操作符。

>以这种方式调用构造函数实际上会经历以下4个步骤：
>1、创建一个新对象
>2、将构造函数的作用域付给新对象（因此this就指向了这个新对象）
>3、执行构造函数中的代码（为这个新对象添加属性）
>4、返回新对象

>这两个对象都有一个constructor属性，指向Person。

>创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型。
>而这正是构造函数胜过工厂模式的地方。

**1、将构造函数当作函数**

>构造函数与其他函数的唯一区别，就在于调用它们的方式不同。
>不过，构造函数毕竟也是函数，不存在定义构造函数的特殊语法。
>任何函数，只要通过new操作符来调用，那它就可以作为构造函数；
>而任何函数如果不通过new操作符来调用，那它跟普通函数也不会有什么两样。

```
// 当作构造函数使用
var person = new Person("Nicholas",29,"Software Engineer");
person.sayName();

// 作为普通函数调用
Person("Greg",27,"Doctor");        // 添加到window，this指向了global对象
window.sayName();

// 在另一个对象的作用域中调用
var o = new Object();
Person.call(o,"Nicholas",29,"Software Engineer");
o.sayName();
```

**2、构造函数的问题**

>**使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。**

>在前面的例子中，person1和person2都有一个名为sayName()的方法，
>但那两个方法不是同一个Function的实例。
>说明白点，以这种方式创建函数，会导致不同的作用域链和标识符解析，
>但创建Function新实例的机制仍然是相同的。
>因此，不同实例上的同名函数的不相等的

```
person1.sayName() == person2.sayName()      // false
```

>创建两个完成同样任务的Function实例的确没有必要。
>可以通过把函数定义转移到构造函数外部来解决这个问题。
>这样做确实解决了两个函数做同一件事的问题，可是新的问题又来了。

>1、在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实。
>2、如果对象需要定义很对方法，那么就要定义很多全局函数，
> **于是我们这个自定义的引用类型就丝毫没有封装性可言了**。

**3、原型模式**

>每个函数都有一个prototype（原型）属性。
>这个属性是一个指针，指向一个对象。
>这个对象的用于是包含可以由特定类型的所有实例共享和属性和方法。
>使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。 

>1、理解原型对象

>无论什么时候，只要创建一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，
>这个属性指向函数的原型对象。
>在默认情况下，所有原型对象都会自动获得一个constructoe属性，
>这个属性是一个指向prototype属性所在函数的指针。

>要明确重要的一点：

    
```
    这个连接存在于实例与构造函数的原型对象之间，
    而不是存在于实例与构造函数之间。

    每个实例都包含一个内部属性，该属性仅仅指向了Person.prototype
    换句话说，实例与构造函数没有直接的关系。
```

>可以通过isPrototypeOf()方法来确定对象之间是否存在这种关系。

```
alert(Person.prototype.isPrototypeOf(person1));
```

>ECMAScript 5增加了一个新方法，叫Object.getPrototypeOf()，
>在所有支持的实现中，这个方法返回[[prototype]]的值，
>可以方便地取得一个对象的原型。
>
>当为对象实例添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性。
>换句话说，添加这个属性只会阻止我们访问原型中的那个属性，但不会修改那个属性。
>即使将这个属性设置为null，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。
>
>不过，使用delete操作符则可以完全删除实例属性，
>从而让我们能够重新访问原型中的属性。

```
delete person1.name;
```

>使用hasOwnProperty()方法可以检测一个属性是存在实例中，还是存在于原型中。
>只在给定属性存在于对象实例中时，才会返回true。 

```
person1.hasOwnProperty("name")
```


**2、原型与in操作符**

>有两种方式使用in操作符：

>    1、单独使用和在for-in循环中使用。
>
>    单独使用时，in操作符会在通过对象能够访问给定属性时返回true。
>
>    同时使用hasOwnProperty()方法和in操作符，就可以确定该属性到底是存在于对象中，
>    还是存在于原型中。
    
```
function hasPrototypeProperty(object , name){
        return !object.hasOwnProperty(name)&&(name in object);
    }   // 只在属性存在于实例中时才返回true
```

>2、在使用for-in循环时，返回的是所有能够通过对象访问的、可枚举的(enumerated)属性。
>   既包括存在于实例中的属性，也包括存在于原型中的属性。
>
>   ECMAScript 5也将constructor和prototype属性的[[Enumerable]]特性设置为false，
>   但并不是所欲浏览器都照此实现。
>
>   要取得对象上所有可枚举的实例属性，可以使用ECMAScript 5的Object.keys()方法来确定对象之间是否存在这种关系。
>   这个方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组
>
>   使用Object.getOwnPropertyName()方法，可以得到所有实例属性，无论是否可枚举。

**3、更简单的原型语法**

>以json的形式赋值

```
Person.prototype = {
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    sayName : fucntion(){
        alert(this.name);
    }
}
```

>最终结果相同，但有一点问题，constructor属性不再指向Person了。
>
>这里的语法本质上完全重写了默认的prototype对象。
>因此，constructor属性也就变成了新对象的constructor属性，
>（指向Object构造函数），不再指向Person函数。
>
>通过constructor已经无法确定对象的类型了。
>
>需要重新设置constructor属性，
>在上面的代码中加入，constructor：Person
>
>但这又带来了一个新的问题，
>    [[Enumerable]]特性随之被设置为了true
>
>默认情况下，原生的constructor属性是不可枚举的。

```
ECMAScript 5中的Object.defineProperty()

// 重设构造函数，只适用于ECMAScript 5兼容的浏览器
Object.defineProperty(Person.prototype,"constructor",{
    enumerated : false,
    value : Person
})
```

**4、原型的动态性**

>对原型对象所做的任何修改都能够立即从实例上反映出来。
>即使是先创建了实例后修改原型也照样如此。
>
>其原因可以归结为实例于原型之间的松散连接关系。
>
>实例与原型之间只不过是一个指针，而非一个副本。

**注意：**

>实例中的指针仅指向原型，而不指向构造函数。
>重写原型对象切断了现有原型与任何之前已存在的对象实例之间的联系。
>它们引用的仍然是最初的原型。

```
function Person{
}

var friend = new Person();

Person.prototype = {
    constructor : Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    sayName : function (){
        alert(this.name);
    }
}

friend.sayName();       // error
```

**5、原生对象的原型**

>原型模式的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型，都是采用这种模式创建的。
>
>所有原生引用类型都在其构造函数的原型上定义了方法。
>
>通过原生对象的原型，不仅可以取得所有默认方法的引用，
>而且也可以定义新方法。
>(不推荐使用，易产生方法名的冲突,而且也会意外的重写原生方法)

```
String.prototype.startWith = function (text){
    return this.indexOf(text) == 0;
};

var msg = "Hello World";
alert(msg.startWith("Hello"));      // true
```

**6、原型对象的问题**

>1、省略了为构造函数传递初始化参数这一环节，结果**所有实现在默认情况下都将取得相同的属性值**。
>2、**由其共享的本性所导致的**。（最大的问题）
>   原型中所有属性是被很多实例共享的。
>   当包含引用类型值的属性是，问题就比较突出了。
>   当一个实例改变了这个引用类型值的属性时，会将原型中的属性值改变。
>   也就是说，其他实例的值也会同时被改变。
>
>   这也就是很少看到有人单独使用原型模式的原因所在。
    
**4、组合使用构造函数模式和原型模式**

>创建自定义类型的最常见方式：就是组合使用构造函数模式与原型模式
>
>构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。

```
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby","Court"];
}

Person.prototype = {
    constructor : Person,
    sayName : function(){
        alert(this.name);
    }
}
```

5、动态原型模式

>可以动过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

```
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    // 检测，也可以使用instanceOf
    if(typeof this.sayName != "function"){
        Person.prototype.sayName = function(){
            alert(this.name);
        }
    }
}
```

**注意：**

>使用动态原型模式时，不能使用对象字面量重写原型。
>前面已经解释过了，如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。

**6、寄生构造函数模式**

**基本思想：**
>创建一个函数。
>该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象；
>但从表面上看，这个函数又很像是典型的构造函数。

```
function Person(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}
```

**关于寄生构造函数模式，有一点需要说明：**
>返回的对象与构造函数或者与构造函数或者与构造函数的原型属性之间没有关系；
>也就是说，构造函数返回的对象与在构造函数外部创建的对象没有什么不同。
>
>不能依赖instanceof操作符来确定对象类型。
>由于存在上述问题，我们建议在可以使用其他模式的情况下，不要使用这种模式。

7、稳妥构造函数模式(单例模式)

>所谓稳妥对象，指的是没有公共属性，而且其方法也不引用this的对象。
>
>稳妥对象最适合在一些安全的环境中(这些环境中会禁止使用this和new)，
>或者在防止数据被其他应用程序(如Mashup程序)改动是使用。

**稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：**

>1、新创建对象的实例方法不引用this
>2、不使用new操作符调用构造函数

```
function Person(name,age,job){
    // 创建要返回的对象
    var o = new Object();

    // 可以在这里定义私有变量和函数

    // 添加方法
    o.sayName = function(){
        alert(name)
    }

    // 返回对象
    return o;
}
```

>安全性高的环境使instanceof同样对其无效。
>上述的对象，除了使用sayName()方法外，没有别的方式可以访问其数据成员。