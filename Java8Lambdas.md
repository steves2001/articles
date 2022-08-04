---
date: 2022-08-04 09:25:36
layout: post
---

## Java lambdas quick start guide
**Since Java 8**, lambda functionality has been introduced that can make your code more readable and simple to edit.  I cannot emphasise how useful this function can be to reduce the time it takes to read and understand your own and others programs.  This is a short summary of some of the uses of lambda functions, a really good but but long guide to Java lambdas has been written at [Jenkov.com](https://jenkov.com/tutorials/java/lambda-expressions.html), should you wish to have greater detail.

### What is a lambda?

Think of a lambda as a function without a name (an anonymous function), this may seem counter intutive, how can a function with no name be more readable?  Stick with this technique it will become clearer as you look at these short examples.  Oracles take on their lambda implementation is below.

> One issue with anonymous classes is that if the implementation of your anonymous class is very simple, such as an interface that contains only one method, then the syntax of anonymous classes may seem unwieldy and unclear. In these cases, you’re usually trying to pass functionality as an argument to another method, such as what action should be taken when someone clicks a button. Lambda expressions enable you to do this, to treat functionality as method argument, or code as data, (Oracle, n.d.)

### Creating a lambda expression

Lambdas generally are used within a command call in a program, so generally you will see them inside another method call or linked to a foreach style loop, this can be difficult to unpick while learning how to write one, the lambda code is shown in isolation initally for readablity:

#### Basic format for a lambda

Normal Java functions/methods are written as:
```
returnDataType functionName(ParamDataType ParamName, ParamDataType ParamName) {
    local function variables 
    function code
    return value if needed
}
```
Lambdas reduce this to:

1. Zero `()`, One `ParamName` or more parameters `(ParamName1, ParamName2)` for the expression.
1. the "->“ token to explain to the compiler that what follows is the lambda expression code block
1. A code block `{}` to contain the code for the function or a single expression without the `{}`.

##### Example lambda definitions

No parameter lambdas

``` Java
() -> { Log.d("Msg", "Lambda called"); }
() -> { return "hi";}
```

A single parameter lambdas parenthesis are optional

``` Java
v -> sellCrypto(v);
(view) -> {// Do some stuff with view}
```

Multiple parameter lambda

``` Java
(num1, num2) -> { return num1 + num2; }
(num1, num2) -> num1 + num2
(num1, num2) -> addNumbers(num1, num2)
```

Using the above notation in code can really reduce the amount of reading and writing required to complete a simple task, not only does this reduce code navigation it lowers the cognitive load when debugging and reviewing the source code.

#### The impact of using lambdas to remove complex syntax of anonymous classes

Still not convinced?  Lets look at how this can shorten code!

In Android development it often neccessary to create objects based on anonymous classes to resolve a simple one method interface for example the View.OnClickListener interface. In an app activity the onCreate method will normall define a set of click listeners using code such as below:

``` Java
buttonBuy.setOnClickListener(new View.OnClickListener() {
    @Override           
    public void onClick(View v) {
        buyCrypto();
        Log.d("Crypto", "Buy button clicked");
    }
});

buttonSell.setOnClickListener(new View.OnClickListener() {
    @Override           
    public void onClick(View v) {
        sellCrypto();           
        Log.d("Crypto", "Sell button clicked");
    }
});
```

Each `new View.onClickListener()` command is creating an anonymous class that implements `public void onClick(View v)` method of the View.OnClickListener interface.

``` Java
class MyOnClickListener implements View.OnClickListener {
    @Override           
    public void onClick(View v) {
        // Handle view click here           
    }
}
```

All this to implement one method!

- **public abstract void onClick (View v)** Called when a view has been clicked.
- **Parameters v** View: The view that was clicked.

Below is the code refactored to use lambdas:
``` Java
buttonBuy.setOnClickListener( v -> {
    buyCrypto();
    Log.d("Crypto", "Buy button clicked");
});

buttonSell.setOnClickListener( v -> {
    sellCrypto();           
    Log.d("Crypto", "Sell button clicked");
});
```

Without the logging in the click event it becomes even shorter:

``` Java
buttonBuy.setOnClickListener(v -> buyCrypto());
buttonSell.setOnClickListener(v -> sellCrypto());
```

> There is a coding debate to be had as to whether it would be better to write the code for buyCrypto() in the lambda, as a **general rule:** if the code is short and not to be called elsewhere it would be better to write it in the lambda, this allows the reader of the code to see the functionality of the lambda in situ. be cautious of doing this if it creates a really large block of code within the containing method as readability can decrease (find the balance).

#### The impact of using lambdas with iteration

Working with data structures is a fundamental of software development and design ever since the concept of an array was implemented in programming languages.  Unfortunately the concept influences how we process these structures, modern progamming techniques try to abstract away iteration to lower the complextity of code by making iteration part of the structure itself.

The code below is pretty standard for outputting an array, it by default has some gotchas:

- An iterator i that can be corrupted by coding errors.
- An arraySize that can be defined incorrectly.
- A complex syntax for referring to the item in the array.

``` Java
for( i=0; i < arraySize; i++ ){
    System.out.println(arrayName[i]);
}
```

Java 8, learned from other language implementations and added a `.forEach()` method to its collections to allow programmers to focus on the processing of the data not the iteration of the data structure (collection).  The `.forEach()` method takes a object created from a class that implements the Consumer interface with one method that recieves current data element (this could be any object) and returns nothing.

``` Java
Consumer<String> stringOutputConsumer = new Consumer<String>() {
    public void accept(String data) {
        System.out.println(data);
    }
};

cryptoCurrencies.forEach(stringOutputConsumer);
```
This can be shortend with classic Java syntax as shown below but it is still lengthy:

``` Java
cryptoCurrencies.forEach(new Consumer<String>() {
    public void accept(String data) {
        System.out.println(data);
    }
};);
```

This is another example of an anonymous class that implements a single method interface, this again matches the lambda requirements, giving in this example very short code:

``` Java
cryptoCurrencies.forEach(data -> System.out.println(data));
```
### Conclusion
Hopefully the conciseness of lambdas are sufficienly exemplified to the point that it convices you the reader that incorporating this technique into your code will ultimately make coding a shorter and more enjoyable experience fro you and your code easier to modify and read in the future.

## References

Android Developers (2022). View.OnClickListener. Available at: https://developer.android.com/reference/android/view/View.OnClickListener.html (Accessed: 4 August 2022).

Jenkov, J. (2022). Java Lambda Expressions, Jenkov.com. Available at: https://jenkov.com/tutorials/java/lambda-expressions.html (Accessed: 4 August 2022).

Oracle. (2022). Lambda Expressions (The Java™ Tutorials > Learning the Java Language > Classes and Objects. Available at: https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html (Accessed: 4 August 2022).
