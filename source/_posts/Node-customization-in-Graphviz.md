title: 在Graphviz中定义节点形状
date: 2015-07-01 11:54:42
tags:
 - graphviz
---
## polygon based shape
默认状态下Graphviz画出的节点都是椭圆形。
{% codeblock %}
digraph G {
   label="default node shape is ellipse";
   A;B;C;D;
}
{% endcodeblock %}
{% asset_img default_node_shape.png %}

<br />
Graphviz提供了多种节点形状，可以通过编辑shape属性改变节点形状。
{% codeblock %}
digraph G {
   label="change node shape";

   node[shape=box];   // change default shape to box
   A;B;               // use default shape
   C[shape=triangle]; // C is triangle
   D[shape=star];     // D is a star
}
{% endcodeblock %}
{% asset_img change_node_shape.png %}

使用node关键字会改变节点的默认值。代码第4行，将默认形状改为方形(box)，这个值将影响它之后出现的所有节点。第5行创建了A、B两个节点。由于他们出现在默认值变为box之后，并且没有指定形状，所以它们都是方形。

像6、7行那样直接指定节点形状则不受默认值影响。

这种直接指定在shape中的形状叫做polygon-based shape。可以在http://www.graphviz.org/content/node-shapes#polygon 看到完整的列表。
{% asset_img polygon_based_shape.png %}

## record based shape
polygen based shape用来画简单框图。graphviz为复杂数据提供了record based shape。这种shape以类似表格的方式绘制节点，特别适合绘制数据结构。

{% codeblock %}
digraph G {
   label="record based shape"

  A[shape=record, label="1|2|3"]
  B[shape=record, label="4|5"]

  A->B
}
{% endcodeblock %}
{% asset_img record_based_shape.png %}

A和B的shape都为record。record节点的结构会根据label的值生成。如A节点label为“1|2|3”会把节点横向三等分，分别填入1、2、3三个元素。
<br />
如果希望把节点纵向分割，可以用{}包围起来。 如 “{1|2|3}”就是一个纵向三等分。
{% codeblock %}
digraph G {
  A[shape=record, label="{1|2|3}"]
}
{% endcodeblock %}
{% asset_img record_based_shape2.png %}
<br />
{}包围起来的元素会改变分割的方向，并支持嵌套。不被包围的元素按横向分割，嵌套一次变成纵向，嵌套两次变回横向，以此类推。例如：
{% codeblock %}
digraph G {
  A[shape=record, label="1|{2|{3|4}}|5"]
}
{% endcodeblock %}
{% asset_img record_based_shape3.png %}

## 为数据命名
先看一幅图
{% asset_img record_based_shape4.png %}

可以控制节点的结构，自然就要控制边的起止。比如上图, left,mid,right是同一个节点的三个元素，要如何控制三个指向I'm xxx的边的起始位置呢。来看代码
{% codeblock %}
digraph G {
  node[shape=record]

  A[label="<l>left|<m>mid|<r>right"]
  B[label="I'm left"]
  C[label="I'm mid"]
  D[label="I'm right"]

  A:l->B
  A:m->C
  A:r->D
}
{% endcodeblock %}
graphviz允许给record中每一个元素命名，元素的名称可以写在<>中。A节点"<l>left|<m>mid|<r>right", 有三个元素left, mid, right分别被命名为l, m, r。这些名字被写在<>中。
于是定义边的时候就可以引用这些名称了。如A:l->B的意思就是从"A中的left元素"画一条边到B。

最后来个稍微复杂一点的例子
{% codeblock %}
digraph G {
  node [shape=record];
  struct1 [shape=record,label=" left|<f1>middle|<f2>right"];
  struct2 [shape=record,label=" one|<f0>two"]; 
  struct3 [shape=record,label="hello\nworld|{ b |{c|<here>d|e}| f}| g | h"];
  struct1:f1 -> struct2:f0;
  struct1:f2 -> struct3:here;
}
{% endcodeblock %}
{% asset_img record_based_shape5.png %}
