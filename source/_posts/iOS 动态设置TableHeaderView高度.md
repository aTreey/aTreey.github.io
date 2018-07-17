## iOS 中动态设置`tableHeaderView`高度

#### 需求

动态的设置`tableHeaderView`的高度

#### 遇到的问题：

- 设置`tableHeaderView` 的`frame`无法实现，有时候会出现`tableView` 中间的 `cell` 不显示
- 有时候会导致`headerView`和`cell`内容重叠

- 通过计算控件高度设置frame方式麻烦

#### 正确的步骤
- 创建HeaderView
- 取出`UITableView`的`tableHeaderView`
- 设置`frame`大小，调整headerView布局
- 重新给`UITableView`的`tableHeaderView`


#### 使用AutoLayout
给`UITableView`增加分类

```
extension UITableView {
    /// set tableHeaderView
    func setTableHeaderView(_ headerView: UIView) {
        headerView.translatesAutoresizingMaskIntoConstraints = false
        self.tableHeaderView = headerView
        // autolayout
        NSLayoutConstraint(item: headerView, attribute: .centerX, relatedBy: .equal, toItem: self, attribute: .centerX, multiplier: 1.0, constant: 0).isActive = true
        NSLayoutConstraint(item: headerView, attribute: .top, relatedBy: .equal, toItem: self, attribute: .top, multiplier: 1.0, constant: 0).isActive = true
        NSLayoutConstraint(item: headerView, attribute: .width, relatedBy: .equal, toItem: self, attribute: .width, multiplier: 1.0, constant: 0).isActive = true
    }
    /// update
    func updateHeaderView() {
        guard let headerView = self.tableHeaderView else { return }
        headerView.layoutIfNeeded()
        let header = self.tableHeaderView
        self.tableHeaderView = header
    }
}
```
