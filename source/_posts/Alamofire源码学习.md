---
title: Alamofire源码学习总结
date: 2017-04-10 15:10:31
tags: Alamofire
---

#### 网络请求时Path和Query 之间是用 ‘？’ 号隔开，后边是传给服务器的参数，GET请求Query是放在URL之后，POST请求是放在Body中

- 如果参数是一个 key-value 形式，Query 格式为：key=value

- 如果参数是一个数组 key = [value1, value2, value3 ….], 

- Query 格式为 key[]=value1&key[]=value2&key[]=value3

- 如果参数是一个字典

```
 key = [“subKey1”:”value1”, “subKey2”:”value2”, “subKey3”:”value3”….], 
```

- Query 的格式为 

```
key[subKey1]=value1&key[subKey2]=value2&key[subKey3]=value3
```

- Alamfire中的编码

```
private func query(_ parameters: [String: Any]) -> String {
        var components: [(String, String)] = []
        for key in parameters.keys.sorted(by: <) {
            let value = parameters[key]!
            components += queryComponents(fromKey: key, value: value)
        }
        return components.map { "\($0)=\($1)" }.joined(separator: "&")
    }
public func queryComponents(fromKey key: String, value: Any) -> [(String, String)] {
        var components: [(String, String)] = [] // 元祖数组
        if let dictionary = value as? [String: Any] { // value 为字典，key[subKey]=value 形式
            for (nestedKey, value) in dictionary {
                components += queryComponents(fromKey: "\(key)[\(nestedKey)]", value: value)
            }
        } else if let array = value as? [Any] { // value为数组， key[]=value 形式 
            for value in array {
                components += queryComponents(fromKey: "\(key)[]", value: value)
            }
        } else if let value = value as? NSNumber { // value为 NSNumber
            if value.isBool {
                components.append((escape(key), escape((value.boolValue ? "1" : "0"))))
            } else {
                components.append((escape(key), escape("\(value)")))
            }
        } else if let bool = value as? Bool { // value 为 Bool 
            components.append((escape(key), escape((bool ? "1" : "0"))))
        } else { // value 为字符串时 直接转义
            components.append((escape(key), escape("\(value)")))
        }
        return components
    }
    
```