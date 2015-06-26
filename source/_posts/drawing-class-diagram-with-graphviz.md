title: 使用graphviz画类图
tags:
  - graphviz
date: 2015-06-26 19:02:07
---
Graphviz是贝尔实验室几位大牛搞出来所谓“所想即所得”的画图软件。说白了就是像写程序一样编写图的"源代码"，用Graphviz把源代码编译成图片。

这种画图的语言叫DOT，试用后感觉很适合程序员。因为我们在画图时大多希望传达的是结构、顺序、关系等逻辑信息。只要能把逻辑表达清楚，方块应该放到什么地方，线应该怎么连并不重要。Graphviz正是这么一种软件——我们只需在DOT中描述图的逻辑。布局、连线等工作全部交给软件去做。

这里记录一种画流程图的方法。


{% asset_img cat_dog.png 图1%}

图1可以用下面的代码生成 
{% asset_link cat_dog.gv %}
{% codeblock %}
digraph G {
    // default attributes
    node[shape=record]
    edge[dir=both, arrowtail=normal, arrowhead=none]

    // node delaration
    Animal[label="{Animal|+eat():void\n+sleep():void}"]
    Cat[label="{Cat|+mew():void}"]
    Dog[label="{Dog|+bark():void}"]

    // edge
    Animal->Cat
    Animal->Dog
}
{% endcodeblock %}


一个DOT源码由三部分组成——graph, node和edge。整幅图就是一个graph, 图中的节点就node，边就是edge。

- 1行生成了一个名称叫做G的有向图(digraph), 大括号里面就是对这个图的描述。
- 3-4行中的node和edge是关键字，分别用来修改节点和边的默认属性。属性出现在中括号中,都是key-value组合。
- 7-9行生成Animal, Cat和Dog三个节点, label属性显示的文字，如果不存在就是显示节点名。
- 12-13生成链接Animal -- Cat, Animal -- Dog 的两条边。

##arrowtail和arrowhead##
第4行edge中, arrowtail和arrowhead都是用来调整边中箭头的显示方式。首先**所有的边都是从tail连接到head的**。
比如Animal->Cat这条边, Animal就是tail, Cat就是head。这一点和日常理解有所不同。
arrortail和arrorhead用来修改tail和head的箭头样式, 可以从官网查看到全部的样式 
[http://www.graphviz.org/content/attrs#karrowType](http://www.graphviz.org/content/attrs#karrowType)
{% asset_img arrow_type.png 箭头样式%}

##dir##
试验后你会发现, 无论arrowtail指定什么值箭头都不会显示。这是因为默认情况下arrowtail就是不显示的，这是由dir属性控制的。
dir可以取四种值forword, back, both和none。分别代表只显示arrowhead, 只显示arrowtail，arrowhead和arrowtail都显示, arrowhead和arrowtail都不显示。如下图
{% asset_img dir.png dir属性%}

<br />
----
最后,node中的shape=record是用来控制节点显示方式的, 比较复杂。后面再讲。
