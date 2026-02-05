Reat Native ref 使用

### State 状态的使用
在父控件中指定,整个生命周期讲不再改变
需求：一段闪烁的文字， Props 属性显示文字内容创建时初始化，State控制随时间变化是否显示，在父控件中

```
class Blink extends Component{
    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {showWithText:true};
    this.state = {}
        setInterval(() => {
            this.setState(previousState => {
                return {showWithText: !previousState.showWithText}
            });
        }, 100);
      }
      render() {
          let display= this.state.showWithText ? this.props.text : ' ';
          return(
              <Text>{display}</Text>
          );
      }
}
```

```
export default class App extends Component<Props> {
    render() {
        return(
            <View style={styles.container}>
                <Blink
                    text='I love to blink'
                />

                <Blink
                  text='猜猜猜'
                />

                <Blink
                    text='Blink is great'
                />
            </View>
        );
    }
}
```

### Props 属性的使用
```
// 定义一个组件
class Greeting extends Component {
    render() {
        return(
            <Text> Hello {this.props.name}!
            </Text>
        );
    }
}
```

```
export default class App extends Component<Props> {
    render() {
        return(
            <View style={styles.container}>
                <Image source={{uri:'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'}}
                       style={{width: 190, height: 110}}
                />
                <Text style={styles.instructions}>
                    使用Props属性
                </Text>
                <View>
                    <Greeting name='React-Native'
                    />
                    <Greeting name='iOS'
                    />
                    <Greeting name='Swift'
                    />
                </View>

            </View>
        );
    }
}
```

#### ref的使用

- 可以理解为组件渲染后指向组件的一个应用，可以通过ref获取到真实的组件（类似指针？）

定义

```
ref='scrollView'

```
获取

```
let scrollView = this.refs.scrollView
let scrollView = this.refs['scrollView']
```
- 可以通过ref访问组件的属性和方法

#### ES6 定时器

- 开启定时器

```
let scrollView = this.refs.scrollView;
// 2.4timer 定时器的写法
this.timer = setInterval(
    ()=>{
        var tempPage = 0;
        //  修改banner索引
        if ((this.state.currentPage+1) >= ImageData.data.length) {
            tempPage = 0;
        } else {
            tempPage = this.state.currentPage+1;
        }

        // 更新状态
        this.setState({
            currentPage: tempPage
        })

        let offSet_x = width * tempPage
        scrollView.scrollResponderScrollTo({x:offSet_x, y:0, animated: true})
    },
    this.state.duration
);
```

- 暂停定时器

```
this.timer && clearInterval(this.timer)
```
