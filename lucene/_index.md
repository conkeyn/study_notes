index(索引)

「索引」，可以作为一个动词，也可以是名词。作为动词的时候，指分析原始文件，分词，存储一系列动作；而作为名词，一般指已经处理好的索引文件。索引，是整个IR系统的核心，先有索引，再有查询。

所有索引文件保存在`Directory`下，一个索引包含很多不同作用的文件，其中有个文件最重要：segments_N，这个文件包含了很多元数据信息，比如当前索引的版本，segments数量。随便写个demo生成索引文件后，一般会有个segments_N文件，其中 N 是32进制的数字格式，如1，2，3 ...。分析下这个二进制文件(我用vim的%!xxd查看的)的数据：

![segment_N文件信息](images/segments.jpg)

我之所称这个文件是元数据信息，因为真正的索引数据不是放在这里，而只是保存了怎么读取索引文件的信息，可以理解成二级索引。上图中可以看到有个`numsegments`字段信息，这表未当前索引有多个 segment 存在，如果有多个，依次加载 segment 信息。每个 segment 文件名称是 segname 保存的。举个例子，如果当前索引有两个segment块，则分别加载读取_0.si和_1.si的信息。记住一点：segment_N文件下保存了1到多个_n.si文件信息。那每个si文件包含哪些信息，同样查看下si二进制文件，有如下格式：

![si文件信息](images/segment.jpg)

