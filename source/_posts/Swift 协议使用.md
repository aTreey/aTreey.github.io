# Swift 协议使用

## 对UIView 扩展

### 按钮/文本框抖动动画

- 制定协议

```
protocol Shakable {}
extension Shakable where Self: UIView {
    func shakeAnimation() {
        let animation = CABasicAnimation(keyPath: "position")
        animation.duration = 0.08
        animation.repeatCount = 5
        animation.autoreverses = true
        animation.fromValue = NSValue(cgPoint: CGPoint(x: self.center.x - 4, y: self.center.y))
        animation.toValue = NSValue(cgPoint: CGPoint(x: self.center.x + 4, y: self.center.y))
        layer.add(animation, forKey: "position")
    }
}
```

- 自定义UI控件并遵守协议

```
class MyButton: UIButton, Shakable {  }
class MySwitch: UISwitch, Shakable {  }
class MyTextField: UITextField, Shakable {  }

```

- 使用

```
let mySwitch = MySwitch()
mySwitch.isOn = true
self.mySwitch = mySwitch
view.addSubview(mySwitch)
mySwitch.translatesAutoresizingMaskIntoConstraints = false
mySwitch.topAnchor.constraint(equalTo: view.bottomAnchor, constant: 10).isActive = true
mySwitch.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 10).isActive = true

// 调用
mySwitch.shakeAnimation()
```


## UITableViewCell使用

### 定义协议

```
protocol ReusableView: class {	}

extension ReusableView where Self: UIView {
    static var reuseIdentifier: String {
        return String(describing: self)
    }
}

``` 

### 对tableView扩展

```
extension UITableView {
    /// 注册cell
    func register<T: UITableViewCell>(T: T.Type) where T: ReusableView {
        print(T.reuseIdentifier)
        self.register(T.self, forCellReuseIdentifier: T.reuseIdentifier)
    }
    
    /// 调用cell
    func dequeueReusableCell<T: UITableViewCell>(_ indexPath: IndexPath) -> T where T: ReusableView {
        print(T.reuseIdentifier)
        return self.dequeueReusableCell(withIdentifier: T.reuseIdentifier, for: indexPath) as! T
    }
}
```

### 用法

- 创建自定义的cell，需要遵守协议

```
class TableProtocolCell: UITableViewCell, ReusableView {
    override var reuseIdentifier: String? {
        return "cell"
    }
    override init(style: UITableViewCellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        self.backgroundColor = .cyan
        self.textLabel?.text = "通过协议和泛型创建的cell"
        self.textLabel?.textColor = .blue
    }
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

class TableProtocolCell2: UITableViewCell, ReusableView {
    override init(style: UITableViewCellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        self.backgroundColor = .red
        self.textLabel?.text = "cell1 - 通过协议和泛型创建的"
        self.textLabel?.textAlignment = .right
    }
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```

- 注册和使用

```
class TableViewProtocolController: UITableViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.register(T: TableProtocolCell.self)
        tableView.register(T: TableProtocolCell2.self)
    }
    
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 10
    }
    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        var cell = UITableViewCell()
        if indexPath.row % 2 == 0 {
            cell = tableView.dequeueReusableCell(indexPath) as TableProtocolCell
        } else {
            cell = tableView.dequeueReusableCell(indexPath) as TableProtocolCell2
        }
        return cell
    }
}

```
