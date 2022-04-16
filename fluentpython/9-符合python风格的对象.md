# 9-符合python风格的对象

## class应具有的功能及实现

1. 直接访问公开成员变量

   1. eg：pig.height

   2. 实现：do nothing

   3. 延伸：python如何定义private？

   4. _xx 可以实现private，即在class外无法普通地访问

   5. __xx的意思则是在private的基础上防止subclass修改

   6. ```
      python的实现方式是把
      __xx --> _$fatherclass__xx，这样子类在集成时就会变成_$subclass_xx，防止更改
      ```

2. 迭代器，拆包 -->可以直接把self当作变量的集合来用

   1. 实现: __iter\_\_

3. 支持repr函数

   1. 实现：__repr\_\_

   2. 作用：提供关于这个对象的一些信息，最好可以是它的生成代码

      即实现eval(rear(obj)) == obj （虽然更好的方法是copy.copy(obj)，这里只是给个例子说明repr的意义：给开发测试提供更多的信息

4. 支持str函数

   1. 实现：__str\_\_
   2. 作用：print(obj)可以输出便于识别的信息（不够repr那么丰富，但可读性更好），如果不实现，python会找到metaclass object的str，最后会返回id(obj)

5. 支持bytes()

   1. 实现：声明class的typecode，实现__bytes\_\_
   2. 作用：返回实例的二进制表示

6. 杂谈

   1. str repr都返回unicode(str类型），而bytes返回bytes类型
   2. format中的 !s代表str(obj)， !r代表repr(obj)， .format()中也支持*
   2. 用dir(directory)展示obj所有的method与变量，就可以看到__xx的转变

