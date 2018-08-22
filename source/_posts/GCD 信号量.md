GCD信号量用法

对于异步网络请求相互依赖问题一般用三种方式解决：
- 网络请求嵌套
- 使用 BlockOperation 
- GCD DispatchGroup
- GCD semaphore（信号量）



### GCD 信号量

- semaphore 值 <= 0 阻塞
- semaphore semaphore.signal() 使信号量值增加
- semaphore.wait() 等待，可设置时长

request -- 3 依赖 request -- 1 和 request -- 2，1 和 2 无需关注顺序

```
 private func gcdSemaphore() {
        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global().async {
            for i in 0...300 {
                print("request -- 1")
            }
            semaphore.signal()
        }
        
        DispatchQueue.global().async {
            for i in 0...200 {
                print("request -- 2")
            }
            semaphore.signal()
        }
        
        
        DispatchQueue.global().async {
            semaphore.wait()
            semaphore.wait()
            for i in 0...160 {
                print("request -- 3")
            }
        }
    }
```

request -- 3 ，request -- 2 和 request -- 1，依次相互依赖

```
private func gcdSemaphore() {
        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global().async {
            for i in 0...300 {
                print("request -- 1")
            }
            semaphore.signal()
        }
        
        DispatchQueue.global().async {
            semaphore.wait()
            for i in 0...200 {
                print("request -- 2")
            }
            semaphore.signal()
        }
        
        
        DispatchQueue.global().async {
            semaphore.wait()
            for i in 0...160 {
                print("request -- 3")
            }
        }
    }

``` 
