# 关于 try catch 用法

**try...catch语句** 将能引发错误的代码放在try块中，并且对应一个相应，然后有异常抛出。

### 语法：

```js

try {
	try_statements
}
[catch (exception_var_1 if condition_1) { // non-standard
	catch_statements_1
}]
...
[catch (exception_ver_2) {
	catch_statements_2
}]
[finally {
	finally_statements
}]

```

* `try_statements` --- 需要被执行的语句
* `catch_statements_1,catch_statements_2` --- 如果在try块里有异常被抛出时执行的语句
* `exception_ver_1,exception_var_2` --- 用于保存关联catch子句的异常对象的标识符
* `condition_1` --- 一个条件表达式
* `finally_statements` --- 在try语句块之后的语句块，无论是否有异常抛出或捕获这些语句都将执行

### 描述

try语句包含了由一个或者多个语句组成的try块，和至少一个catch子句或者一个finally子句的其中一个，或者两个兼容，下面是三种形式的try声明：

1. try...catch
2. try...finally
3. try...catch.finally

catch语句中包含要执行的语句，当try语句抛出错误时。也就是，你想让try语句中的内容成功，如果没有成功，你想控制接下来发生的事情，这时你可以在catch语句中实现。如果有在try块中有任何一个语句（或者从try块中调用的函数）抛出异常，控制立即专享catch子句。如果在try块中没有异常抛出，这catch子句将会跳过。

finallly子句在try块和catch块之后执行但是在下一个try声明之前执行。无论是否有异常抛出或者是否被捕获它总是执行。

你可以嵌套一个或者更多的try语句。如果内部的try语句没有catch子句，那么将会进入保卫它的try语句的catch子句。

你也可以用try语句处理javascript异常。参见[JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)了解更多关于JavaScript异常的信息。

### 无条件的catch子句

当单个的无条件的catch子句被使用时，当有异常抛出时则将会进入catch块执行语句。例如，当下面的代码发生异常时，控制将会转向catch子句。

```js

try {
	throw "myException"; // generates an exception
}

catch ( e ) {
	// statements to handle any exceptions
	logMyErrors( e ); // pass exception object to error handler
}

```

### 条件catch子句

*非标准，该特性是非标准的，请尽量不要在生产环境中使用它！*

你也可以使用一个或者更多条件catch子句来处理特定的异常。在这种情况下，当异常抛出时将会合适的catch子句中。在下面的代码中，try块的代码可能会抛出三种异常：`TypeError`,`RangeError`,和`EvalError`。当一个异常抛出，控制将会进入与其对应的catch语句。若果这个异常不是特定的，那么控制将转义到非条件catch子句。

当用一个非条件catch子句和一个或多个条件语句时，非条件catch子句必须放在最后。否则当到达条件语句之前所有的异常将会被条件语句拦截。

提醒：这个功能不符合ECMAscript规范

```js

try {
	myroutine(); // mat throw three types of exceptions
} catch ( e if e instanceof TypeError ) {
	// statements to handle TypeError exceptions
} catch ( e if e instanceof RangeError ) {
	// statements to handle RangeError exceptions
} catch (e if e instanceof EvalError) {
    // statements to handle EvalError exceptions
} catch (e) {
    // statements to handle any unspecified exceptions
    logMyErrors(e); // pass exception object to error handler
}

```

下面用符合ECMAscript规范的简单的JavaScript来编写相同的“条件子句”（显然更加冗长，但是可以在任何地方运行）：

```js

try {
    myroutine(); // may throw three types of exceptions
} catch (e) {
    if (e instanceof TypeError) {
        // statements to handle TypeError exceptions
    } else if (e instanceof RangeError) {
        // statements to handle RangeError exceptions
    } else if (e instanceof EvalError) {
        // statements to handle EvalError exceptions
    } else {
       // statements to handle any unspecified exceptions
       logMyErrors(e); // pass exception object to error handler
    }
}

```

### 异常标识符

当try块中的抛出一个异常，`exception_var`（e.g.the `e`in`catch (e)`）用来保存被抛出声明指定的值。你可以用这个标识符来获取关于被抛出异常的信息。

这个标识符时catch子句内部的。换言之，当进入catch子句的标识符创建，catch子语句执行完毕后，这个表示符不在可用。

### finally 从句

finally子句在try块和catch块之后执行但是在一个try声明之前执行。无论是否有异常抛出它总是执行。如果有异常抛出finally子句将被执行，这个声明没有catch子句处理异常。

你可以使用finally子句使你的脚本结束地更加优雅。例如，你可能需要释放你的脚本已经用过的资源。下面的示例打开一个文件然后执行使用文件的语句（服务端的javascript允许你访问的文件）。如果文件打开时有一个异常抛出，finally子句关闭文件在脚本结束之前。

```js

openMyFile()
try {
   // tie up a resource
   writeMyFile(theData);
}
finally {
   closeMyFile(); // always close the resource
}

```



