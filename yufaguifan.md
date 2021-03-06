# YAML语法规范

### 组织结构

YAML文件可以由一或多个文档组成\(也即相对独立的组织结构组成\),文档间使用**“---”**\(三个横线\)在每文档开始作为分隔符.同时,文档也可以使用“...”\(三个点号\)作为结束符\(可选\).

```
---
invoice: 34843
date: "2001-01-23"
bill-to:
  given: Chris
  family: Dumars
  address:
    lines: '458 Walkman Dr.Suite #292'
    city: Royal Oak
    state: MI
    postal: 48046
...
---
ship-to:
  given: Chris
  family: Dumars
  address:
    lines: '458 Walkman Dr.Suite #292'
    city: Royal Oak
    state: MI
    postal: 48046
...
```

* 如果只是单个文档,分隔符**“---”**可以省略
* 文档结束符**“...”**不是必须的.\(但是对于网络传输或者流来说,作为明确结束的符号,有利于软件处理\)

#### YAML支持的三种数据结构

* 标量:单个的、不可再分的值,又称为纯量\(scalars\)
* 序列:一组按次序排列的值,又称为序列\(sequence\)/列表\(list\)
* 对象:键值对的集合,又称为映射\(mapping\)/哈希\(hashes\)/字典\(dictionary\)

### 编写规范

* **规范一** : 文档使用 Unicode 编码作为字符标准编码,例如 UTF-8
* **规范二** : 使用“\#”来表示注释内容

```
# 客户订单
date: 2015-02-01
customer:
  - name: Jai
items:
  - no: 1234 # 订单号
  - descript: cpu
```

* **规范三** : 使用空格作为嵌套缩进工具.通常建议使用两个空格缩进,不建议使用tab\(甚至不支持\)
* **规范四** : 序列表示\(数组\)

  * 使用“-”\(横线\) + 单个空格表示单个列表项

    ```
    --- # 文档开始
    - 第一章 简介
    - 第二章 设计目录
    ```

  * 使用"\[\]"表示一组数据

    ```
    --- # 文档开始
    color:[blue, red, green]
    ```

  * 组合表示.每个结构都可以嵌套组成复杂的表示结构.

    ```
    --- # 文档开始
    - [blue, red, green] # 列表项本身也是一个列表
    - [Age, Bag]
    - site: {osc:www.oschina.net, baidu: www.baidu.com} # 这里是同{键值表}组合表示
    ```

* **规范五** : 键值表\(对象\)

  * 使用“:”\(冒号\) + 空格表示单个键值对

    ```
    # 客户订单
    date: 2015-02-01
    customer:
      - name: Jai
    items:
      - no: 1234 # 订单号
      - descript: cpu
      - price: ￥800.00
    ```

  * 使用"{}"表示一个键值表

    ```
    # 客户订单
    date: 2015-02-01
    customer:
      - name: Jai
    items: {no: 1234, descript: cpu, price: ￥800.00}
    ```

  * "?"问号 + 空格表示复杂的键.当键是一个列表或键值表时,就需要使用本符号来标记.

    ```
    # 使用一个列表作为键
    ? [blue, reg, green]: Color
    # 等价于
    ? - blue
      - reg
      - gree
    : Color
    ```

  * **组合表示**.每个结构都可以嵌套组成复杂的表示结构.

    ```
    Color:
      - blue
      - red
      - green
    # 相当于 (也是 JSON 的表示)
    {Color: [blue, red, green]}

    div:
      - border: {color: red, width: 2px}
      - background: {color: green}
      - padding: [0, 10px, 0, 10px]

    # 使用缩进表示的键值表与列表项
    items:
      - item: cpu
        model: i3
        price: ￥800.00
      - item: HD
        model: WD
        price: ￥450.00

    # 上面使用 “-” 前导与缩进来表示多个列表项,相当于下面的JSON表示
    items: [{item:cpu, model:i3, price:￥800.00}, {item:HD, model:WD, price: ￥450.00}]
    ```

* **规范六** : 文本块

  * 使用 “\|” 和文本内容缩进表示的块,保留块中已有的回车换行,相当于段落块.

    ```
    # 注意 ":" 与 "|" 之间的空格
    --- # 文档开始
    abc: |
      aabbcc eee
      dd
    ```

  * 使用 “&gt;” 和文本内容缩进表示的块,将块中回车替换为空格,最终连接成一行.

    ```
    # 注意 ":" 与 ">" 之间的空格，另外可以使用空行来分段落
    --- # 文档开始
    abc: >
      aabbcc eee
      dd
    ```

  * 使用定界符“”\(双引号\)、‘’\(单引号\)或回车表示的块,最终表示成一行.

    ```
    yaml: # 使用回车的多行,最终连接成一行.
      "JSON的语法其实是YAML的子集,
      大部分的JSON文件都可以被YAML的解释器解释."
    ```

    * 单引号之中如果还有单引号,必须连续使用两个单引号转义

      ```
      str: 'labor''s day' 
      ```

    * 单引号和双引号都可以使用,单引号不会对特殊字符转义

    * 字符串可以写成多行,从第二行开始,必须有一个单空格缩进.换行符会被转为空格

      ```
      str: 这是一段
        多行
        字符串
      ```

    * `+`表示保留文字块末尾的换行,`-`表示删除字符串末尾的换行

      ```
      s1: |
        Foo

      s2: |+
        Foo


      s3: |-
        Foo
      ```

    * 字符串之中可以插入HTML标记

      ```
      message: |

        <p style="color: red">
          段落
        </p>
      ```

* **规范七** : 数据类型的约定

  * 对一些常用数据类型的表示格式进行了约定

    ```
    integer: 12345     # 整数标准形式
    octal: 0o34        # 八进制表示，第二个是字母 o
    hex: 0xFF          # 十六进制表示

    float: 1.23e+3     # 浮点数
    fixed: 13.67       # 固定小数
    minmin: -.inf      # 表示负无穷
    notNumber: .NaN    # 无效数字

    null:              # 空值,也可以使用~表示null
    boolean: true      # 布尔值
    string: '12345'    # 字符串

    date: 2015-08-23   # 日期
    datetime: 2015-08-23T02:02:00.1z  # 日期时间
    iso8601: 2015-08-23t21:59:43.10-05:00  # iso8601 日期格式
    spaced: 2015-08-23 21:59:43.10 -5      # ?
    ```

  * “!”\(叹号\)显式指示类型,或自定义类型标识.单叹号通常是自定义类型,双叹号是内置类型

    ```
    isString: !!str 2015-08-23     # 强调是字符串不是日期数据
    picture: !!binary |            # Base64  图片
         R0lGODlhDAAMAIQAAP//9/X
         17unp5WZmZgAAAOfn515eXv
         Pz7Y6OjuDg4J+fn5OTk6enp
         56enmleECcgggoBADs=
    # 下面是内置类型
    !!int               # 整数类型
    !!float             # 浮点类型
    !!bool              # 布尔类型
    !!str               # 字符串类型
    !!binary            # 也是字符串类型
    !!timestamp         # 日期时间类型
    !!null              # 空值
    !!set               # 集合
    !!omap, !!pairs     # 键值列表或对象列表
    !!seq               # 序列，也是列表
    !!map               # 键值表

    #下面是一些例子：
    --- !!omap
    - Mark: 65
    - Sammy: 63
    - Key: 58

    --- !!set           # 注意，“?”表示键为列表，在这里列表为 null
    ? Mark
    ? Sammy
    ? Key

    # 下面是自定义的类型或标识
    %TAG ! tag:clarkevans.com,2002:   # % 是指令符号
    --- !shape
    # Use the ! handle for presenting
    # tag:clarkevans.com,2002:circle
    - !circle
      center: &ORIGIN {x: 73, y: 129}
      radius: 7
    - !line
      start: *ORIGIN
      finish: { x: 89, y: 102 }
    - !label
      start: *ORIGIN
      color: 0xFFEEBB
      text: Pretty vector drawing.
    ```

* **规范八** : 锚点与引用,定义数据的复用

  * 第一步 - 使用"&"定义数据锚点\(即要复制的数据\)
  * 第二部 - 使用"\*"引用上述锚点数据\(即数据复制到的位置\)
    ```
    ---
    hr:
      - Mark McGwire
      # Following node labeled SS
      - &SS Sammy Sosa            # 定义要复制的数据
    rbi:
      - *SS                       # 这里是数据复制目标
      - Ken Griffey
    ```

    `<<`表示合并到当前数据

    ```
    defaults: &defaults
      adapter:  postgres
      host:     localhost

    development:
      database: myapp_development
      <<: *defaults

    test:
      database: myapp_test
      <<: *defaults
    ```



