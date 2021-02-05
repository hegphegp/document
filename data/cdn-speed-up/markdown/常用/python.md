# python

* python只有四种数据：整数，长整数、浮点数和复数；java则有char，short，byte，boolean，int，long，float，double类型
* Java的集合类包括list、map和set，而Python更强调字典（对于Java的map）和列表（对应Java的list），淡化了set这个概念。对于列表的处理方法大同小异，Python的遍历里面有个印象深刻的[-1]下标，代表集合最后一个，这样避免了下标溢出。
* Python可以用单引号''或双引号""来表示一个字符串，也可以用三引号来表示一个多行字符串
* Python 中的__init__()方法类似与Java中的构造函数，但是Python需要在__init__()函数中显示指明，调用时不用显示进行self传递
*  Python中可以使用str（）或repr（）函数来实现对象的序列化，Java中通过toString()方法来实现对象的序列化


* self.send_chan, self.recv_chan = TqChan(self), TqChan(self)  # 消息收发队列
* 变量A，变量B = 表达式A，表达式B