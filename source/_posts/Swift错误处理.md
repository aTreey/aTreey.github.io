Swift 中错误处理


### assert 断言

只在debug 模式下程序会自动退出

```
public func assert(_ condition: @autoclosure () -> Bool, _ message: @autoclosure () -> String = default, file: StaticString = #file, line: UInt = #line)
```

### assertionFailure 断言错误

只在debug 模式下程序会自动退出, 没有条件

```
public func assertionFailure(_ message: @autoclosure () -> String = default, file: StaticString = #file, line: UInt = #line)
```

### precondition 

releas 模式程序也会退出

```
public func precondition(_ condition: @autoclosure () -> Bool, _ message: @autoclosure () -> String = default, file: StaticString = #file, line: UInt = #line)
```

### throw 