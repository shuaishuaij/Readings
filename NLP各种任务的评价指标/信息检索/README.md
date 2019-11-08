

# P@10

P@10意思是：返回前10个结果的精确度。P英文为Precision。

# MAP
MAP因为为Mean Average Precision，就是说对一个叫Average Precision（AP）的东西取平均值。检测一个系统的性能，常用多个不同种类的查询对它进行测试，每个查询的结果都能计算出一个AP值，把所有AP取平均值就是系统的MAP。

那么问题就变成了如何对一次查询结果计算AP值。AP值其实是对P@n的一个扩展。上述的P@10是吧n固定为10，而AP的计算是平均P@1,P@2......P@n所有的值。
