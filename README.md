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
* **无所作为的代码**，如下代码，
```
if (!!(Object.keys(labelCounts).includes(label))) {

}
```
其中**!!**是不必要的
