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
    ```
        // 块级表达式
        // let friends = 10;
        case friends
            when 0: p You have no friends
            when 1: p You have one friend
            default: p You have #{friends} friend
    ```
## Code语法
    ### Unbuffered Code——以-开头
        ```
            ul
                - for(let i = 0; i < 3; i++)
                    li item
        ```
        ```
            // let items = ['Bill', 'Gate', 'Jack', 'Smith'];
            ul
                each item in items
                    li= item
        ```
    ### Buffered Code——以=开头，接受js表达式
        ```
            p= 'This is paragraph ' + '<strong>strong</strong>'  => <p>This is paragraph <strong>strong</strong></p>
        ```
    ### Unescaped Code——以!=开头，和Buffered Code区别是!=解析字符串中的html元素
        ```
            p!= 'This is paragraph ' + '<strong>strong</strong>'  => <p>This is paragraph strong</p>
        ```
## Comments
    ### 单行注释
        ```
            // line comment  => 会在html中渲染
            //- line comment  => 不会在html中渲染
        ```
    ### 多行注释
        ```
            //
                multiple
                comment
            //- 
                multiple
                comment
        ```
## Conditional
    ### if语句
        ```
            if condition1
                statemeng1
            else if condition2
                statement2
            else
                statement3
        ```
    ### unless语句——相当于if !condition
        ```
            unless condition
                statement
        ```
## 文档声明——doctype html
## 导入文档
    include filename，导入的文件可以是pug文件，也可以是其他类型文件
## 模板继承
    block定义一个块，可以在子模板中复写，子模板可以通过extends继承父模板
    ```
        // 父
        block contents
            p This is comment paragraph.  // 如果子模板中没有复写，则默认显示此行，如果复写则覆盖此行
    ```
    ```
        // 子
        extends parent
        block append/prepend contents  // 可以通过append(追加)、prepend(前加)来不覆盖父模板中定义的内容，此时block关键字可选
            p This is child template
    ```