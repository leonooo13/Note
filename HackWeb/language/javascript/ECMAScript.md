ECMAScript
========================================

简介
----------------------------------------
ECMAScript是一种由ECMA国际通过ECMA-262标准化的脚本程序设计语言，它往往被称为JavaScript或JScript。简单的，可以认为ECMAScript是JavaScript的一个标准，但实际上后两者是ECMA-262标准的实现和扩展。

版本
----------------------------------------
1997年6月，首版发布。1998年6月，进行了格式修正，以使得其形式与ISO/IEC16262国际标准一致。1999年12月，引入强大的正则表达式，更好的词法作用域链处理，新的控制指令，异常处理，错误定义更加明确，数据输出的格式化及其它改变。而后由于关于语言的复杂性出现分歧，第4版本被放弃，其中的部分成为了第5版本及Harmony的基础。

2009年12月，第五版发布，新增“严格模式（strict mode）”，澄清了许多第3版本的模糊规范，并适应了与规范不一致的真实世界实现的行为。增加了部分新功能，如getters及setters，支持JSON以及在对象属性上更完整的反射。

2015年6月，第6版发布，最早被称作是 ECMAScript 6（ES6），添加了类和模块的语法，迭代器，Python风格的生成器和生成器表达式，箭头函数，二进制数据，静态类型数组，集合（maps，sets 和 weak maps），promise，reflection 和 proxies。

2016年6月，ECMAScript 2016（ES2016）发布，引入 ``Array.prototype.includes`` 、指数运算符、SIMD等新特性。

2017年6月，ECMAScript 2017（ES2017）发布，多个新的概念和语言特性。

2018年6月，ECMAScript 2018 （ES2018）发布包含了异步循环，生成器，新的正则表达式特性和 rest/spread 语法。

ES6 特性
----------------------------------------
- ``const`` / ``let``
- 模板字面量
- 解构
    - ``[a, b] = [10, 20]``
- 对象字面量简写法
- ``for...of`` 循环
- ``...xxx`` 展开运算符
- 可变参数
- 箭头函数
- 默认参数函数
- 默认值与解构
- 类