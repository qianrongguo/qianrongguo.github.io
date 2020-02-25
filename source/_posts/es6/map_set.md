Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。
Maps 和 Objects 的区别
1. 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
2. Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
3. Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
4. Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

#### Map 中的 key
key 是字符串
key 是对象
key 是函数
key 是 NaN(虽然 NaN 和任何值甚至和自己都不相等(NaN !== NaN 返回true)，NaN作为Map的键来说是没有区别的)

#### Map 的迭代
for...of
forEach()

#### Map 对象的操作
Map 与 Array的转换
1. Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
2. 	使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组

Map 的克隆:(Map 对象构造函数生成实例，迭代出新的对象。)

Map 的合并:(合并两个 Map 对象时，如果有重复的键值，则后面的会覆盖前面的)

### Set 对象
Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

#### Set 中的特殊值
1. Set 对象存储的值总是唯一的，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：
2. +0 与 -0 在存储判断唯一性的时候是恒等的，所以不重复；
3. undefined 与 undefined 是恒等的，所以不重复；
4. NaN 与 NaN 是不恒等的，但是在 Set 中只能存一个，不重复。

类型转换
	Array 
1. Array 转 Set
2. 	 用...操作符，将Set转Array
3. 	 String转set(set中toString方法不能讲Set转换成String)
Set 对象作用

	数组去重(先new Set(),再讲Set转换为Array)
	并集`(new Set([...a, ...b]);)`
	交集`(new Set([...a].filter(x => b.has(x))))`
	差集`(new Set([...a].filter(x => !b.has(x))))`

 

