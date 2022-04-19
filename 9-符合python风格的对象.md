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

## classmethod and staticmethod

它们是做什么的？：对class(animal)而不是instance(cat)操作的方法

classmethod常见作用：提供一个新的创建instance的方法，因为python中不存在overload，classmethod默认第一个参数是cls，不管写没写（比如说假如只提供*args，那么第一个就是cls）

## FOrmat

### how to use format

"{ a:0.2f } is great".format(a=yxb)

- b binary
- d decimal
- e 0.2e scientific mode
- f : float, 0.2f
- o,x octual heximal
- .1%
- %H M S 

### 定义__format\_\_作用

实现format("self", "spetial notation")的功能

## hashlize

实现hash的三层阶梯

1. 不可变：把变量self.x变成self._x
2. 实现__eq\_\_
3. 实现hash，通常可以使用reduce(operator.xor, hashes)

## 保护装置vs安全装置

__x 只能是保护装置，因为如果要强行修改修改的话还是可以实现的。

约定_x为private，但python不会给予特殊处理

## slot类属性节省空间

默认情况下python用dict储存实例属性。slot会让解释器用tuple储存属性

在类中定义 __slots__ 属性之后， 实例不能再有 __slots__ 中所列名称之外 的其他属性。这只是一个副作用，不是 __slots__ 存在的真正原因。

每个子类都要定义 __slots__ 属性，因为解释器会忽略继承的 __slots__ 属性。

如果不把 '__weakref__' 加入 __slots__ ，实例就不能作为弱引用的目标。

实例只能拥有 __slots__ 中列出的属性，除非把 '__dict__' 加入 __slots__ 中（这样做 就失去了节省内存的功效）。

## 覆盖类属性

1. 子类覆盖

类属性是公开 的，因此会被子类继承，于是经常会创建一个子类，只用于定制类的数据属性。 Django 基 于类的视图就大量使用了这个技术。具体做法如示例 9-14 所示。

```python
>>> from vector2d_v3 import Vector2d

>>> class ShortVector2d(Vector2d): ... typecode = 'f'
```

2. 实例覆盖（只覆盖该事务）

这也说明了我在 Vecto2d.__repr__ 方法中为什么没有硬编码 class_name 的值， 而是使用 type(self).__name__ 获取
