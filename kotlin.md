https://kotlinlang.org/docs/kotlin-tour-hello-world.html#string-templates



这个函数包含了好几个知识点

这是一个有String类的作为参数  返回(Int)->Int lambda表达式的函数至于那个等号  是因为如果函数只有一句话  就这么写

```
You can remove the curly braces {} and declare the function body using the assignment operator =. When you use the assignment operator =, Kotlin uses type inference, so you can also omit the return type. The sum() function then becomes one line:
```



```
fun toSeconds(time: String): (Int) -> Int = when (time) {
    "hour" -> { value -> value * 60 * 60 }
    "minute" -> { value -> value * 60 }
    "second" -> { value -> value }
    else -> { value -> value }
}

```



# Extension functions

这算是个特性

## Extension properties

However, extension properties in Kotlin do not have backing fields. 