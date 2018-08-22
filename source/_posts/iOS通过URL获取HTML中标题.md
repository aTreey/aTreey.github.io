iOS通过URL获取HTML中标题

### 获取标题

##### String扩展一个方法

```
private let patternTitle = "(?<=title>).*(?=</title)"
extension String {
    static func parseHTMLTitle(_ urlString: String) -> String? {
        let url = URL(string: urlString)!
        do {
            let html = try String(contentsOf: url, encoding: .utf8)
            let range = html.range(of: patternTitle, options: String.CompareOptions.regularExpression, range: html.startIndex..<html.endIndex, locale: nil)
            guard let tempRange = range else { return "" }
            let titleStr = html[tempRange]
            return String(titleStr)
        } catch {
            return ""
        }
    }
}

```

##### iOS 中简单正则表达式

iOS 中 正则表达式中的\ 需要使用 \\\\ 来表示

    // 获取
	let testStr = "我到底是[iOS]开发还是[产品]经理"
	
    // 获取 "[" 和 "]" 之间的所有内容, 左边[ 起一直到 右边 ] 结束的所有内容 
    let pattern1 = "\\[.*\\]" // [iOS]开发还是[产品]
    
    // 获取 "[" 和 "]" 之间的所有内容，包括[]
    let pattern2 = "\\[.*?\\]"	// [iOS]
    
    // 获取第一个 "[" 和 "]" 之间的所有内容 不包括[] 
    let pattern3 = "(?<=\\[).*?(?=\\])" // iOS
    
	// 获取html中 title 标签中内容
    let pattern5 = "(?<=title>).*(?=</title)" 
    

##### 获取范围截取子串
	
	let testRange1 = testStr.range(of: pattern1, options: String.CompareOptions.regularExpression, range: testStr.startIndex..<testStr.endIndex, locale: nil)
        let result1 = String(testStr[testRange1!])
        
        
    let testRange2 = testStr.range(of: pattern2, options: String.CompareOptions.regularExpression, range: testStr.startIndex..<testStr.endIndex, locale: nil)
    let result2 = String(testStr[testRange2!])
    
    
    let testRange3 = testStr.range(of: pattern3, options: String.CompareOptions.regularExpression, range: testStr.startIndex..<testStr.endIndex, locale: nil)
    let result3 = String(testStr[testRange3!])
    
    print(result1) 
    print(result2) 
    print(result3) 

##### 正则表达式
通过URL解析html中的标题

- 获取html字符串
- 正则匹配
- 生成字符串Range
- 截取字符串


匹配一次直接返回结果

```
// 生成表达式
let regular = try NSRegularExpression(pattern: pattern5, options: .caseInsensitive) 
 

// 开始匹配  
let matchResult1 = regular.firstMatch(in: html, options: NSRegularExpression.MatchingOptions.reportProgress, range: NSMakeRange(0, html.count))

// 获取匹配结果范围
let matchRange = matchResult1?.range
let range = Range(matchRange!)!
    
// 生成String.Index 类型范围 
let startIndex = html.index(html.startIndex, offsetBy: range.lowerBound)
let endIndex = html.index(html.startIndex, offsetBy: range.upperBound)
let range3 = Range(uncheckedBounds: (lower: startIndex, upper: endIndex))

// 截取字符串
let result1 = String(html[range3])
print(result1)
```

全部匹配后返回结果时一个集合

```
let matchResult2 = regular.matches(in: html, options: .reportProgress, range: NSMakeRange(0, html.count))
            matchResult2.forEach { (result) in
let range = Range(result.range)!
    
let startIndex = html.index(html.startIndex, offsetBy: range.lowerBound)
let endIndex = html.index(html.startIndex, offsetBy: range.upperBound)
    
let subStr2 = html[Range(uncheckedBounds: (startIndex, endIndex))]
print(String(subStr2))
}

```

将匹配到的内容提还

```
let str3 = regular.stringByReplacingMatches(in: html, options: .reportProgress, range: NSMakeRange(0, html.count), withTemplate: "====")

```



