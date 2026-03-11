
# 使用 Graphviz 绘图


### 相关资料
- [官方](http://www.graphviz.org/documentation/)
- [使用DOT语言和Graphviz绘图(翻译)](https://casatwy.com/shi-yong-dotyu-yan-he-graphvizhui-tu-fan-yi.html)
- [参考手册](https://graphs.grevian.org/)
- [Graphviz 简易教程](https://blog.zengrong.net/post/graphviz-brief/)
- [graphviz-visual-editor](http://magjac.com/graphviz-visual-editor/)


- https://app.diagrams.net/?src=about
- https://mermaid-live.nodejs.cn/edit
- https://tool.lu/markdown/

### 安装

`yum install 'graphviz*'`
`pip install graphviz`

- centos 避免中文乱码问题 安装楷体字体
`yum install cjkuni-ukai-fonts`


dot 默认布局方式，主要用于有向图
neato 基于spring-model(又称force-based)算法
twopi 径向布局
circo 圆环布局
osage
fdp 用于无向图
sfdp 用于无向图
patchwork 方形树


- dot 命令使用

格式：dot -T<type> -o<outfile> <infile.dot>    --> dot -Tpng -o xx.png xx.dot
输入文件是<infile.dot>，生成的格式由<type>指定，生成的文件是<outfile>。

其中-T<type>包括
```
-Tps (PostScript),
-Tsvg -Tsvgz (Structured Vector Graphics), 
-Tfig (XFIG  graphics), 
-Tmif  (FrameMaker graphics),
-Thpgl (HP pen plotters),
-Tpcl (Laserjet printers),
-Tpng -Tgif (bitmap graphics),
-Tdia (GTK+ based diagrams),
-Timap (imagemap files for httpd servers for each node or edge  that  has a non-null "href" attribute.),
-Tcmapx (client-side imagemap for use in html and xhtml).
-Gdpi=300 # 图像分辨率


twopi -Gdpi=300 -Tpng jsfc_timeout.dot -o jsfc_timeout.png

dot -Tpng -Gdpi=300 x.dot -o x.png
dot -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png
circo -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png
fdp -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png
sfdp -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png 
osage -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png 
neato -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png 
twopi -Gdpi=300 -Tpng 连衣裙.dot -o 连衣裙.png

```

### 0x0. dot 基本语法

- [Node, Edge and Graph Attributes](https://graphviz.org/doc/info/attrs.html)


#### graph setting
- `bgcolor="transparent";` 背景透明
- `fontname="Microsoft YaHei";` 字体
- `fontsize=18;` 字体大小
- `rankdir=LR;` 布局方向 
    - `LR` 从左到右 `RL`与之相反
    - `TB` 从上到下 `BT`与之相反
- `splines=none;` 隐藏边


#### splines 属性
- 无边 `splines=""` 或 `splines=none`
- 样条线 `splines=spline` 或 `splines=true`
- 直线段 `splines=line` 或 `splines=false`
- 折线 `splines=polyline`
- 曲弧线 `splines=curved`
- 正直线 `splines=ortho`


#### Basic style settings for nodes
```
olid		dashed		dotted	
bold		rounded		diagonals	
filled		striped		wedged
```

#### Basic style settings for edges

- `invis` 可以隐藏边
    - `style=invis`

```
olid		dashed	
dotted		bold	
invis
```

#### Basic style settings for clusters
```
solid		dashed		dotted		bold	
rounded		filled		striped
```

#### dir Type 边的箭头
- dir=forward

```
forward
back	
both	
none
```




#### arrow Type

- `arrowhead`
- `arrowtail`
- `arrowsize`

```
normal		inv	
dot		invdot	
odot		invodot	
none		tee	
empty		invempty	
diamond		odiamond	
ediamond		crow	
box		obox	
open		halfopen	
vee	
```


#### label相关
- `label`
- 边的`label` 可以有
    -`a->b [taillabel="从a开始", headlabel="到b结束", label="中间"]`

#### 文字相关 
- `fontcolor` 文字颜色
- `fontsize` 文字大小
- `fontname` 字体颜色

#### 轮廓相关
- `color`
- `penwidth`

#### 填充颜色
- [https://graphviz.org/doc/info/colors.html](https://graphviz.org/doc/info/colors.html)
- `fillcolor`
    - darkgreen
    - skyblue
    - skyblue3
    - lightpink3
    - lightblue2
    - lightskyblue2
    - cyan3
    - cyan4
    - lightcyan3
    - chocolate1
    - chocolate2
    - deepskyblue
    - deepskyblue3
    - deepskyblue4
    - darkseagreen1
    - darkseagreen2
    - darkseagreen3
    - palegreen3
    - bisque3
    - tan
    - burlywood
    - burlywood3
    - darkolivegreen3
    - gold1
    - chartreuse3
    - seagreen2
    - aquamarine4
    - honeydew3
    - indianred3
    - lightsalmon
    - #39e581ff
    - #8139e5aa

#### 风格
- `style`
    - `filled`

#### 布局方向
- `rankdir` 
    - `LR` 从左到右布局   
    - `RL` 从右到左布局
    - `BT` 从下往上布局
    - `TB` 从上往下布局
    

#### port 属性 节点或边的方向
```text
n	north 北
ne	north east
e	east 东
se	south east 东南
s	south 南
sw	south west 西南
w	west 西
nw	north west 西北
c	center 中部
_	任意

```
- edge的 tailport 属性
    - `a -> b [tailport = e]`
    - `a->b:e`
值
    

    
#### 节点形状
- [https://graphviz.org/doc/info/shapes.html](https://graphviz.org/doc/info/shapes.html)
- `shape`
    - `box` 矩形
    - `box3d` 立方体
    - `polygon` 多边图
    - `ellipse` 椭圆
    - `oval`  椭圆形；卵形
    - `circle` 圆形
    - `point` 点
    - `egg` 蛋
    - `triangle` 三角形
    - `plaintext`  纯文本
    - `diamond` 菱形
    - `trapezium` 梯形
    - `parallelogram` 平行四边形
    - `house` 房子
    - `start` 五角星
    - `pentagon` 五角形
    - `hexagon` 六角形
    - `septagon` 七边形
    - `octagon` 八边形
    - `doublecircle` 双圆形
    - `doubleoctagon` 双八边形
    - `tripleoctagon` 三重八边形
    - `Mrecord` 圆角的矩形
    - `note` 文档形
    - `folder` 文件夹
    - `tab` tab组件
    - `cylinder` 圆柱体
    - `record` 数据结构
    - `component` 部件


    
```digraph
graph graphname {
    dpi=300;
    rankdir=LR;  //Rank Direction Left to Right
    bgcolor="transparent";//背景透明
    edge [color="#636260", style="filled", arrowhead="normal", arrowtail="normal", fontsize="14"];
    
    a [shape=box,]
    
    a -- b; 
    b -- c;
    b -- d;
    d -- a;
}
```


### 数据结构

```digraph
digraph structs {
    node[shape=record]
    struct1 [label="<f0> left|<f1> mid\ dle|<f2> right"];
    struct2 [label="{<f0> one|<f1> two\n\n\n}" shape=Mrecord];
    struct3 [label="hello\nworld |{ b |{c|<here> d|e}| f}| g | h"];
    struct1:f1 -> struct2:f0;
    struct1:f0 -> struct3:f1;
}
```




### 流程图 demo

```digraph
digraph cgi{
    fontname="Consolas";
    fontsize=10;

    node [shape="Mrecord", fontname="Consolas", fontsize=10];
    edge [color="#4788C7", fontname="Consolas", fontsize=10];

    chrome [shape="none", image="browser-icon-chrome-resized.png", label=""];
    firefox [shape="none", image="browser-icon-firefox-resized.png", label=""];

    apache [label="Apache httpd", style="filled", color="#FBD20A"];
    cgi [label="CGI script", style="filled", color="#4BB449"];

    node [color="#E7463D"];
    static [label="Static Resources", shape="note"];
    dynamic [label="Dynamic Resources", shape="note"];
    database[label="Database", shape="cylinder"];
    remote [label="Remote Services", shape="box3d"]

    firefox -> apache [label="HTTP Request"];
    chrome -> apache [label="HTTP Request"];
    apache -> cgi [label="fork-exec"];

    cgi -> static [label="Access content"];
    cgi -> dynamic [label="Access content"];
    dynamic -> remote [label="RESTful/SOAP"];
    cgi -> database[label="Access Database for content"];
}
```

### 状态迁移图/有限状态机

```digraph
digraph finite_state_machine {
	rankdir=LR;
	
	size="8,5"
	
	node [shape = doublecircle]; 
	LR_0 LR_3 LR_4 LR_8;
	
	node [shape = circle];
	LR_0 -> LR_2 [ label = "SS(B)" ];
	LR_0 -> LR_1 [ label = "SS(S)" ];
	LR_1 -> LR_3 [ label = "S($end)" ];
	LR_2 -> LR_6 [ label = "SS(b)" ];
	LR_2 -> LR_5 [ label = "SS(a)" ];
	LR_2 -> LR_4 [ label = "S(A)" ];
	LR_5 -> LR_7 [ label = "S(b)" ];
	LR_5 -> LR_5 [ label = "S(a)" ];
	LR_6 -> LR_6 [ label = "S(b)" ];
	LR_6 -> LR_5 [ label = "S(a)" ];
	LR_7 -> LR_8 [ label = "S(b)" ];
	LR_7 -> LR_5 [ label = "S(a)" ];
	LR_8 -> LR_6 [ label = "S(b)" ];
	LR_8 -> LR_5 [ label = "S(a)" ];
}




digraph finite_state_machine {
    rankdir=LR;
    size="8,5"
    node [shape = circle];
    S0 -> S1 [ label = "Lift Nozzle" ]
    S1 -> S0 [ label = "Replace Nozzle" ]
    S1 -> S2 [ label = "Authorize Pump" ]
    S2 -> S0 [ label = "Replace Nozzle" ]
    S2 -> S3 [ label = "Pull Trigger" ]
    S3 -> S2 [ label = "Release Trigger" ]
}



```


### 数据流示意图
```digraph
digraph dfd{    
    node[shape=record]
    store1 [label="<f0> left|<f1> Some data store"];
    proc1 [label="{<f0> 1.0|<f1> Some process here\n\n\n}" shape=Mrecord];
    enti1 [label="Customer" shape=box];
    store1:f1 -> proc1:f0;
    enti1-> proc1:f0;
}


```


### 时序图 demo 
```digraph
digraph G {
    rankdir="LR";
    node[shape="point", width=0, height=0];
    edge[arrowhead="none", style="dashed"]
    
    {
        rank="same";
        edge[style="solided"];
        LC[shape="plaintext",shape="egg",style="filled",color="skyblue3"];
        LC -> step00 -> step01 -> step02 -> step03 -> step04 -> step05;
    }
    
    {
        rank="same";
        edge[style="solided"];
        Agency[shape="plaintext",shape="egg",style="filled",color="skyblue3"];
        Agency -> step10 -> step11 -> step12 -> step13 -> step14 -> step15;
    }
    
    {
        rank="same";
        edge[style="solided"];
        Agent[shape="plaintext",shape="egg",style="filled",color="skyblue3"];
        Agent -> step20 -> step21 -> step22 -> step23 -> step24 -> step25;
    }
    
    step00 -> step10 [label="sends email new custumer", arrowhead="normal"];
    step11 -> step01 [label="declines", arrowhead="normal"];
    step12 -> step02 [label="accepts", arrowhead="normal"];
    step13 -> step23 [label="forward to", arrowhead="normal"];
    step24 -> step14;
    step14 -> step04 [arrowhead="normal"];
}


```



### uml demo
```digraph

digraph G {
    # 定义全局属性
    fontname = "Courier New"
    # fontsize = 8
    # 从下往上
    rankdir = BT
    # 定义节点属性
    node [
            shape = "record" # 矩形，默认是椭圆
            color = "blue" # 边框蓝色
    ]

    # 定义边的属性
    edge [fontsize = 9]
    # 换行符是\l，而要新建一个新的单元格，则需要是用|。{}里面的是内容
    Reportable [label = "{Reportable | + getSummary() : Map\<String, Integer\> | + getDetail() : Map\<String, Integer\> | + isDetailVisible() : boolean}"]
    # 特殊字符要转义
    LineCounter [label = "{LineCounter | + count(String line) : boolean | + getType() : String}"]
    CharCounter [label = "{CharCounter | + count(Character c) : boolean | + getType() : String}"]

    AbstractCharCounter [label = "{AbstractCharCounter | characterMap : Map\<Character, Integer\> | + count(Character c) : boolean | + getSummary() : Map\<String, Integer\> | +getDetail() : Map\<String, Integer\> }"]
    AbstractLineCounter [label = "{AbstractLineCounter |  + count(String line) : boolean | + getSummary() : Map\<String, Integer\> | +getDetail() : Map\<String, Integer\> }"]

    PredicateCharacter[label = "{Predicate\<Character\> | + apply(Character c) : boolean}"]
    PredicateString[label = "{Predicate\<String\> | + apply(String line) : boolean}"]

    BlankCharCounter[label = "{BlankCharCounter | + apply(Character c) : boolean | + getType() : String | + isDetailVisible() : boolean }"]
    ChineseCharCounter[label = "{ChineseCharCounter | - chinesePattern : Pattern | + apply(Character c) : boolean | + getType() : String | + isDetailVisible() : boolean }"]
    LetterCharCounter[label = "{LetterCharCounter | - chinesePattern : Pattern | + apply(Character c) : boolean | + getType() : String | + isDetailVisible() : boolean }"]
    NumberCharCounter[label = "{NumberCharCounter | + apply(Character c) : boolean | + getType() : String | + isDetailVisible() : boolean }"]
    LineNumberCounter[label = "{LineNumberCounter | + apply(Character c) : boolean | + getType() : String | + isDetailVisible() : boolean }"]
   

    parentInterface [label = "parent interface" color = "green" style=filled]
    childInterface [label = "child interface" color = "green" style=filled]
    abstractClass [ label = "abstract class : implement some methods using the abstract methods" color = "green" style=filled]
    specificClass [ label = "specific class : implement all unimplemented methods" color = "green" style=filled]

    LineProcessor [label = "{LineProcessor\<List\<Reportable\>\>}" ]
    ReportableLineProcessor [ label = "{ReportableLineProcessor | + ReportableLineProcessor() | + processsLine(String line) : boolean | + getResult() : List\<Reportable\>}"]

    # 定义在同一层
    {rank = same; parentInterface; Reportable; LineProcessor}
    {rank = same; childInterface; LineCounter; CharCounter; PredicateCharacter; PredicateString}
    {rank = same; abstractClass; AbstractLineCounter; AbstractCharCounter;}
    {rank = same; specificClass; LineNumberCounter; BlankCharCounter; ChineseCharCounter; LetterCharCounter; NumberCharCounter; ReportableLineProcessor}
    
    # 箭头为空心，接口之间的继承
    LineCounter -> Reportable[arrowhead="empty"]
    CharCounter -> Reportable[arrowhead="empty"]
    
    AbstractCharCounter -> CharCounter[arrowhead="empty"]
    AbstractCharCounter -> PredicateCharacter[arrowhead="empty"]
    
    AbstractLineCounter -> LineCounter[arrowhead="empty"]
    AbstractLineCounter -> PredicateString[arrowhead="empty"]

    # 实现类的UML
    BlankCharCounter -> AbstractCharCounter[arrowhead="empty", style="dashed"]
    ChineseCharCounter -> AbstractCharCounter[arrowhead="empty", style="dashed"]
    LetterCharCounter -> AbstractCharCounter[arrowhead="empty", style="dashed"]
    NumberCharCounter -> AbstractCharCounter[arrowhead="empty", style="dashed"]
    LineNumberCounter -> AbstractLineCounter[arrowhead="empty", style="dashed"]

    ReportableLineProcessor -> LineProcessor[arrowhead="empty", style="dashed"]
  
}

```


### 对象继承关系
```digraph

digraph obj{
    node[shape=record];
    rankdir="BT";

    teacher [label = "{<f0> Teacher|<f1> \n  |<f2> \n   }"];
    course [label = "{<f0> Course|<f1> \n  |<f2> \n   }"];
    student [label = "{<f0> Student|<f1> \n  |<f2> \n   }"];
    lesson [label = "{<f0> Lesson |<f1> \n  |<f2> \n   }"];
    tutorial [label = "{<f0> Tutorial|<f1> \n  |<f2> \n   }"];
    assessment[label = "{<f0> Assessment|<f1> \n  |<f2> \n   }"];
    coursework [label = "{<f0> Coursework|<f1> \n  |<f2> \n   }"];
    exam [label = "{<f0> Exam|<f1> \n  |<f2> \n   }"];

    {rank=same; teacher course student}

    teacher->course [dir="forward",arrowhead="none",arrowtail="normal",headlabel="1",taillabel="1.."];
    student->course [dir="forward",arrowhead="none",arrowtail="normal",headlabel="1",taillabel="1.."];
    lesson->course [dir="forward",arrowhead="diamond",arrowtail="normal"];
    tutorial->course [dir="forward",arrowhead="diamond",arrowtail="normal"];
    assessment->course [dir="forward",arrowhead="diamond",arrowtail="normal"];
    coursework->assessment;
    exam->assessment;
}

```


### 实体关系
```digraph

digraph ER{
    node[shape=box];
    Book;
    Customer;
    Loan;
    {rank=same;Book,Customer,Loan}
    Book->Loan[dir="forward",arrowhead="crow",arrowtail="normal"];
    Customer->Loan[dir="forward",arrowhead="crow",arrowtail="normal"];
}

```


### 国学  
```digraph

digraph structs {
    node[shape=record]
    dpi=300;
    rankdir=LR;  //Rank Direction Left to Right
    // bgcolor="transparent";//背景透明
    
    
    dizhi [label="{{{<5>巳}|<4>辰|<3>卯|{<2>寅} }|{ {<6>午|<7>未} |{}|{<1>丑|<0>子}}|{{<8>申}|<9>酉|<10>戌|{<11>亥} }}"];
    
    bagua [label="<0>震巽|<1>离|<2>坤艮|<3>乾兑|<4>坎"]
    fangwei [label="{{东南|东|东北} | {南|中|北}  |  {西南|西|西北}}"]
    
    
    wuxing [label="<0>木|<1>火|<2>土|<3>金|<4>水"]
    tiangan [label="<0>甲乙|<1>丙丁|<2>戊己|<3>庚辛|<4>壬癸"]
    siji [label="<0>春|<1>夏|<2>长夏|<3>秋|<4>冬"]
    wei [label="<0>酸|<1>苦|<2>甘|<3>辛|<4>咸"]
    se [label="<0>青|<1>红|<2>黄|<3>白|<4>黑"]
    sheng [label="<0>宫|<1>商|<2>角|<3>徵|<4>羽"]
    wufang [label="<0>东|<1>南|<2>中|<3>西|<4>北"]
    wuzhang [label="<0>肝|<1>心|<2>脾|<3>肺|<4>肾"]
    liufu [label="<0>胆|<1>小肠|<2>胃|<3>大肠|<4>三焦、膀胱"]
    wuguan [label="<0>目|<1>舌|<2>口|<3>鼻|<4>耳"]
    xingti [label="<0>筋|<1>脉|<2>肉|<3>皮毛|<4>骨"]
    wuye [label="<0>泪|<1>汗|<2>涎|<3>涕|<4>唾"]
    qingxu [label="<0>怒|<1>喜|<2>思|<3>悲|<4>恐"]
    
    
    wuxing:0 -> tiangan: 0 -> siji: 0 -> wei: 0  -> se: 0 -> sheng: 0 -> wufang: 0
    wuxing:1 -> tiangan: 1 -> siji: 1 -> wei: 1  -> se: 1 -> sheng: 1 -> wufang: 1
    wuxing:2 -> tiangan: 2 -> siji: 2 -> wei: 2  -> se: 2 -> sheng: 2 -> wufang: 2
    wuxing:3 -> tiangan: 3 -> siji: 3 -> wei: 3  -> se: 3 -> sheng: 3 -> wufang: 3
    wuxing:4 -> tiangan: 4 -> siji: 4 -> wei: 4  -> se: 4 -> sheng: 4 -> wufang: 4
    
    wufang: 0 -> wuzhang: 0 -> liufu:0 -> wuguan: 0  -> xingti: 0 -> wuye: 0 -> qingxu: 0
    wufang: 1 -> wuzhang: 1 -> liufu:1 -> wuguan: 1  -> xingti: 1 -> wuye: 1 -> qingxu: 1
    wufang: 2 -> wuzhang: 2 -> liufu:2 -> wuguan: 2  -> xingti: 2 -> wuye: 2 -> qingxu: 2
    wufang: 3 -> wuzhang: 3 -> liufu:3 -> wuguan: 3  -> xingti: 3 -> wuye: 3 -> qingxu: 3
    wufang: 4 -> wuzhang: 4 -> liufu:4 -> wuguan: 4  -> xingti: 4 -> wuye: 4 -> qingxu: 4
    
    
}

```




