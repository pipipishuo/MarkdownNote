https://kotlinlang.org/docs/kotlin-tour-hello-world.html#string-templates


[Kotlin Docs | Kotlin Documentation](https://kotlinlang.org/docs/home.html)

[Kotlin Multiplatform quickstart | Kotlin Multiplatform Documentation](https://kotlinlang.org/docs/multiplatform/quickstart.html#get-help) 


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



# Trailing lambdas



1当lamda表达式是函数的最后一个参数时  就可以把他用大括号去写

2如果lamda表达式只有一个参数的时候 可以用it关键字代替就不用写箭头啥的了

示例代码

```

fun main() {
    repeat(3) {
        println("Iteration $it")
        println("Iteration ")
    }
}
```





# 导航



感觉cmp这可以用这个NavDisplay

滑动处理非常不错的示例

```
@Composable
fun SwipeBackHandler(
    backStack: MutableList<Any>, // 来自 Navigation 3 的 backStack
    enabled: Boolean = true,
    onSwipeBack: () -> Unit = { backStack.removeLastOrNull() }
) {
    if (!enabled) return

    val maxSwipeDistance = 200.dp
    val threshold = 0.3f // 滑动超过30%即触发返回

    Box(
        modifier = Modifier
            .fillMaxSize()
            .pointerInput(Unit) {
                // 检测水平拖动
                detectHorizontalDragGestures(
                    onDragStart = { offset ->
                        // 可选：只有从屏幕左边缘开始的拖动才处理
                        // 这里简化为所有水平拖动
                    },
                    onDragEnd = {
                        // 滑动结束时，如果超过了阈值，执行返回
                        if (backStack.size > 1) { // 确保不是首页
                            onSwipeBack()
                        }
                    }
                ) { change, dragAmount ->
                    // 这里可以处理滑动过程中的动画（例如页面跟随移动）
                    change.consume()
                }
            }
    )
}

// 在你的 NavDisplay 中使用
@Composable
fun MyAppNavHost() {
    val backStack = remember { mutableStateListOf<Any>(Home) }

    NavDisplay(
        backStack = backStack,
        onBack = { backStack.removeLastOrNull() }
    ) {
        entry<Home> {
            // 首页不需要左滑返回，可以禁用或显示提示
            SwipeBackHandler(backStack, enabled = false)
            HomeScreen()
        }
        entry<Detail> { entry ->
            // 详情页启用左滑返回
            SwipeBackHandler(backStack, enabled = true) {
                backStack.removeLastOrNull()
            }
            DetailScreen(id = entry.argument.id)
        }
    }
}
```

[Jetpack Compose 和 Compose Multiplatform 还有 KMP 的关系今天刚好看到官方发布 - 掘金](https://juejin.cn/post/7462776363734564916) 
