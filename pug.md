## Attribute
    tagName(attribute1=value1 attribute2=value2) text
        `a(href='xxx' class='xxx')xxx`
    三元一次表达式: tagName(attribute1=boolean ? value1 : value2)
        `a(class=boolean ? value1 : value2)xxx`
    模板字符串: tagName(attribute=`${variable}`)
        `strong(data-time=`${time}`)xxx`
    字符串: tagName(attribute='xxx ' + variable)
        `strong(class='btn ' + test)` => <strong class='btn test'>
    防止跨脚本攻击包含特殊字符应转义(用!=代替=)
        ```
            div(class='<code>')  => div(class='&lt;code&gt;')
            div(class!='<code>')  => div(class='<code>')
        ```
    布尔属性——默认为true，可以指定为true或false
        ```
            form(method='GET' action='http://localhost:3000/login')
                label
                    input(type='radio' name='ball' value='basketball' checked)
                    | 篮球
                br
                label
                    input(type='checkbox' name='friuts' value='apple' checked=true)
                    | 苹果
                label
                    input(type='checkbox' name='friuts' value='bananan' checked=false)
                    | 香蕉
                br
                select(name='select1')
                    option(value='one') 选择1
                    option(value='two' selected) 选择2
                    option(value='three') 选择3
                input(type='submit' value='提交')
        ```
    style属性——可以使用对象或者字符串
        ```
            tagName(style='background: xxx; width: xxx')
            tagName(style={ background: xxx, width: xxx})
        ```
    class属性——可以使用字符串或变量或数组
        ```
            tagName(class='p1')  => <tagName class='p1'>
            tagName(class=p2)  => <tagName class='p2'>  // p2: 'p2'
            tagName(class=arr)  => <tagName class='p2 p3 p4'>  // arr: ['p2', 'p3', 'p4']
            tagName(class=arr class=p5)  => <tagName class='p2 p3 p4 p5'>  // arr: ['p2', 'p3', 'p4']  p5: 'p5'
            tagName(class=arr class=['p9'])  => <tagName class='p2 p3 p4 p9'>  // arr: ['p2', 'p3', 'p4']
        ```
    class简写——省略标签则默认是一个div元素
        ```
            a.button  => <a class='button'>
            .button  => <div class='button'> 
        ```
    id简写——同class简写一致
    &attributes(obj/variable)——将对象的键值对迭代到元素的属性上
        ```
            div&attributes({ 'data-time': 1111, 'data-test': 'test'})  => <div data-time= '1111' data-test='test'>
        ```
## Case语法——跟js的switch语法一致
    ```
        // let friends = 10;
        case friends
            when 0
                p You have no friends
            when 1
                p You have one friend
            default
                p You have #{friends} friends
    ```