# 第6章 重构基本结构
## 6.1 该如何开始重构？
### 6.1.1 重命名
最简单的重构就是**重命名没有意义的事物**。
以下属于不好的名字:
* **拼写错误的单词**，比如ttal并不是一个真正的单词，而是一个写错了的单词，开发者写错了，漏了一个字母o，本来是要写total的，重构的时候我们就要将ttal替换回total;
* **变量中含有数字**，比如有一个song_11的变量名(var song_11 = [])，其变量名中包含了数字并且采用了**蛇形命名(使用下划线连接)**的方式，这并不是好的命名变量的方式，推荐使用**驼峰命名**的方式，并且不包含数字，我们可以用**songEleven**来替换掉song_11，但是从其定义来看，song_11是一个空的数组，我们可以使用**blankSong**来定义似乎更语义化些;
* **函数和变量名不是采用驼峰命名**，比如有一个button_status的变量，我们可以用驼峰命名的方式，将button_status替换为**buttonStatus**;
* **使用单字母定义变量**，比如，我们在循环中经常使用i和j来表示循环的索引，使用单字母定义变量有着很大的缺点，就是不利于搜索，因为很多变量名都含有i和j这个两个字母，搜索起来非常的不方便，我们可以使用**index和innerIndex**等来替换掉i和j;

### 6.1.2 无用代码
**如果是不需要的代码就把它去掉**。
无用的代码分为:
* **死代码**，比如定义了一个变量或者函数，但是**没有任何一个地方使用到了该变量或者调用了该函数**，我们可以将这些不被使用的变量和函数移除；
* **空白符**，我们的代码中**留存很多空白行，通常不会对程序的功能产生什么影响**，通常为了提高程序的阅读性，**留一行空白行是必要的**，但是多于1行空白行会带来不必要的麻烦；
* **无所作为的代码**，如下代码:<br/>
```
if (!!(Object.keys(labelCounts).includes(label))) {

}
```
其中 **!!** 是不必要的，因为includes()函数返回的就是一个布尔值了，不需要使用 **!!** 把它转换成布尔值，同时if()条件中也会自动进行强制转换为布尔值，所以 **!!** 属于多余的操作，可以将 **!!** 去除；<br/>
**JavaScript中的6个假值: false、undefined、null、NaN、0、''(空字符串)** <br/>
再比如: <br/>
**JavaScript中并不区分整数和浮点数，他们都是数字类型，所以在进行数字计算的时候，如果计算结果是一个小数，我们不需要显示将整数形式写成小数的形式去计算, 10/3 和10.0/3在JavaScript中结果都是一样的，写成10.0就显得多余了** <br/>

**条件中的重复**也是一种无用的代码，如:<br/>
```
if (dog.weight > 40) {
  buyFood('big bag');
  dog.feed();
} else {
  buyFood('small bag');
  dog.feed();
}
//其实无论dog的weight多大，我们都要进行feed()操作，所以我们可以改写为:
if (dog.weight > 40) {
  buyFood('big bag');
} else {
  buyFood('small bag');
}
 dog.feed();
```
* **魔数**，所谓魔数，就是**硬编码到程序中的数字**，这些硬编码的数字会给程序的维护带来非常多不必要的麻烦，所以称他们为魔，如:<br/>
第一，魔数是一个纯数字，没有任何的解释，看到这个数字并不能知道这个数字是干嘛的;<br/>
第二，魔数直接硬编码在代码的各处，如果需要改变这个数字，那么将要改变多出<br/>
所以我们**需要定义一个有意义的变量名来保存这个魔数，方便阅读和维护**，如: <br/>
```
function getGravity(mass){
  return 9.8 * mass;
}
// 这是一个获取重力的函数，在地球上重力加速度是9.8，但是在其他星球上则不是，所以不方便改变，可以改成
function getGravity(mass){
  let gSpeed = 9.8;
  return gSpeed * mass;
}
```
**应该使用哪种循环？**<br/>
对于传统的for、while循环，需要对索引进行大量的维护，但是**其实很多时候我们并不关心索引的变化，所以我们可以采用for...of或者for...in循环来代替传统的for循环**，当然有的时候我们又需要使用到索引，所以我们可以使用更加灵活的循环**forEach循环** <br/>

