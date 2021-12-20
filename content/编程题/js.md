# [返回主页](https://github.com/yisainan/web-interview/blob/master/README.md)

<b><details><summary>1. 如何获取浏览器 URL 中查询字符串中的参数？</summary></b>

参考答案：

方法一：(基础版)

``` js
function getQueryString() {
    var sHref = window.location.href;
    var args = sHref.split("?");
    if (args[0] == sHref) {
        // 没有参数，直接返回空即可
        return "";
    }
    var arr = args[1].split("&");
    var obj = {};
    for (var i = 0; i < arr.length; i++) {
        var arg = arr[i].split("=");
        obj[arg[0]] = arg[1];
    }
    return obj;
}
var href = getQueryString();
console.log(href["categoryId"]);
```

方法二：(正则版, URL 存在#则不适用)

``` js
function getQueryString(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) return unescape(r[2]);
    return null;
}
console.log(getQueryString("categoryId"));
```

方法三：(正则升级版)

``` js
function getQueryString(name) {
    // 未传参，返回空
    if (!name) return null;
    // 查询参数：先通过search取值，如果取不到就通过hash来取
    var after = window.location.search;
    after = after.substr(1) || window.location.hash.split("?")[1];
    // 地址栏URL没有查询参数，返回空
    if (!after) return null;
    // 如果查询参数中没有"name"，返回空
    if (after.indexOf(name) === -1) return null;
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    // 当地址栏参数存在中文时，需要解码，不然会乱码
    var r = decodeURI(after).match(reg);
    // 如果url中"name"没有值，返回空
    if (!r) return null;
    return r[2];
}
console.log(getQueryString("categoryId"));
```

[参与互动](https://github.com/yisainan/web-interview/issues/549)

</details>

<b><details><summary>2. js 实现一个打点计时器</summary></b>

问题描述：

1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console. log 一个数字，每次数字增幅 1
2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作
3、第一个数需要立即输出

参考答案：

``` js
// 实现法一（setTimeout()方法）：

function count(start, end) {
    if (start <= end) {
        console.log(start++);
        st = setTimeout(function() {
            count(start, end);
        }, 100);
    }
    return {
        cancel: function() {
            clearTimeout(st);
        }
    };
}
count(1, 10);

// 实现法二（setInterval()方法）：

function count(start, end) {
    console.log(start++);
    var timer = setInterval(function() {
        if (start <= end) {
            console.log(start++);
        }
    }, 100);
    return {
        cancel: function() {
            clearInterval(timer);
        }
    };
}
count(1, 10);
```

知识点：
setTimeout()方法用于在指定的毫秒数后调用函数或计算表达式。
语法：setTimeout(code, millisec)
注意：setTimeout() 只执行 code 一次。如果要多次调用，请使用 setInterval() 或者让 code 自身再次调用 setTimeout()。

setInterval() 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。
语法：setInterval(code , millisec[, "lang"])
setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

[参与互动](https://github.com/yisainan/web-interview/issues/550)

</details>

<b><details><summary>3. 用 js 实现一个标准的排序算法</summary></b>

参考答案：

一. 冒泡排序

> 它是最慢的排序算法之一，但也是一种最容易实现的排序算法。
> 之所以叫冒泡排序是因为使用这种排序算法排序时，数据值会像气泡一样从数组的一端漂浮到另一端。假设正在将一组数字按照升序排列，较大的值会浮动到数组的右侧，而较小的值则会浮动到数组的左侧。之所以会产生这种现象是因为算法会多次在数组中移动，比较相邻的数据，当左侧值大于右侧值时将它们进行互换。

``` js
function BubbleSort(array) {
    var length = array.length;
    for (var i = length - 1; i > 0; i--) {
        //用于缩小范围
        for (var j = 0; j < i; j++) {
            //在范围内进行冒泡，在此范围内最大的一个将冒到最后面
            if (array[j] > array[j + 1]) {
                var temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
        console.log(array);
        console.log("-----------------------------");
    }
    return array;
}

var arr = [10, 9, 8, 7, 7, 6, 5, 11, 3];
var result = BubbleSort(arr);
console.log(result);
/*
[ 9, 8, 7, 7, 6, 5, 10, 3, 11 ]
-----------------------------
[ 8, 7, 7, 6, 5, 9, 3, 10, 11 ]
-----------------------------
[ 7, 7, 6, 5, 8, 3, 9, 10, 11 ]
-----------------------------
[ 7, 6, 5, 7, 3, 8, 9, 10, 11 ]
-----------------------------
[ 6, 5, 7, 3, 7, 8, 9, 10, 11 ]
-----------------------------
[ 5, 6, 3, 7, 7, 8, 9, 10, 11 ]
-----------------------------
[ 5, 3, 6, 7, 7, 8, 9, 10, 11 ]
-----------------------------
[ 3, 5, 6, 7, 7, 8, 9, 10, 11 ]
-----------------------------
[ 3, 5, 6, 7, 7, 8, 9, 10, 11 ]
*/
```

二. 选择排序

> 选择排序从数组的开头开始，将第一个元素和其他元素进行比较。检查完所有元素后，最小的元素会被放到数组的第一个位置，然后算法会从第二个位置继续。这个过程一直进行，当进行到数组的倒数第二个位置时，所有数据便完成了排序。

``` js
function SelectionSort(array) {
    var length = array.length;
    for (var i = 0; i < length; i++) {
        //缩小选择的范围
        var min = array[i]; //假定范围内第一个为最小值
        var index = i; //记录最小值的下标
        for (var j = i + 1; j < length; j++) {
            //在范围内选取最小值
            if (array[j] < min) {
                min = array[j];
                index = j;
            }
        }
        if (index != i) {
            //把范围内最小值交换到范围内第一个
            var temp = array[i];
            array[i] = array[index];
            array[index] = temp;
        }
        console.log(array);
        console.log("---------------------");
    }
    return array;
}

var arr = [1, 10, 100, 90, 65, 5, 4, 10, 2, 4];
var result = SelectionSort(arr);
console.log(result);
/*
[ 1, 10, 100, 90, 65, 5, 4, 10, 2, 4 ]
---------------------
[ 1, 2, 100, 90, 65, 5, 4, 10, 10, 4 ]
---------------------
[ 1, 2, 4, 90, 65, 5, 100, 10, 10, 4 ]
---------------------
[ 1, 2, 4, 4, 65, 5, 100, 10, 10, 90 ]
---------------------
[ 1, 2, 4, 4, 5, 65, 100, 10, 10, 90 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 100, 65, 10, 90 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 10, 65, 100, 90 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 10, 65, 100, 90 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 10, 65, 90, 100 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 10, 65, 90, 100 ]
---------------------
[ 1, 2, 4, 4, 5, 10, 10, 65, 90, 100 ]
*/
```

三. 插入排序

> 插入排序有两个循环。外循环将数组元素挨个移动，而内循环则对外循环中选中的元素进行比较。如果外循环中选中的元素比内循环中选中的元素小，那么数组元素会向右移动，为内循环中的这个元素腾出位置。

``` js
function InsertionSort(array) {
    var length = array.length;
    for (var i = 0; i < length - 1; i++) {
        //i代表已经排序好的序列最后一项下标
        var insert = array[i + 1];
        var index = i + 1; //记录要被插入的下标
        for (var j = i; j >= 0; j--) {
            if (insert < array[j]) {
                //要插入的项比它小，往后移动
                array[j + 1] = array[j];
                index = j;
            }
        }
        array[index] = insert;
        console.log(array);
        console.log("-----------------------");
    }
    return array;
}

var arr = [100, 90, 80, 62, 80, 8, 1, 2, 39];
var result = InsertionSort(arr);
console.log(result);
/*
[ 90, 100, 80, 62, 80, 8, 1, 2, 39 ]
-----------------------
[ 80, 90, 100, 62, 80, 8, 1, 2, 39 ]
-----------------------
[ 62, 80, 90, 100, 80, 8, 1, 2, 39 ]
-----------------------
[ 62, 80, 80, 90, 100, 8, 1, 2, 39 ]
-----------------------
[ 8, 62, 80, 80, 90, 100, 1, 2, 39 ]
-----------------------
[ 1, 8, 62, 80, 80, 90, 100, 2, 39 ]
-----------------------
[ 1, 2, 8, 62, 80, 80, 90, 100, 39 ]
-----------------------
[ 1, 2, 8, 39, 62, 80, 80, 90, 100 ]
-----------------------
[ 1, 2, 8, 39, 62, 80, 80, 90, 100 ]
*/
```

四. 希尔排序

> 希尔排序(Shell's Sort)是插入排序的一种又称“缩小增量排序”（Diminishing Increment Sort），是直接插入排序算法的一种更高效的改进版本。希尔排序是非稳定排序算法。该方法因D.L.Shell于1959年提出而得名。

```js
function shellSort(arr) {
  let len = arr.length;
  // gap 即为增量
  for (let gap = Math.floor(len / 2); gap > 0; gap = Math.floor(gap / 2)) {
    for (let i = gap; i < len; i++) {
      let j = i;
      let current = arr[i];
      while(j - gap >= 0 && current < arr[j - gap]) {
        arr[j] = arr[j - gap];
        j = j - gap;
      }
      arr[j] = current;
    }
    console.log(arr);
    console.log("-----------------------");
  }
}

var arr = [3,5,7,1,4,56,12,78,25,0,9,8,42,37];
shellSort(arr);

/*
[3, 5, 0, 1, 4, 42, 12, 78, 25, 7, 9, 8, 56, 37]
-----------------------
[1, 4, 0, 3, 5, 8, 7, 9, 25, 12, 37, 42, 56, 78]
-----------------------
[0, 1, 3, 4, 5, 7, 8, 9, 12, 25, 37, 42, 56, 78]
-----------------------
*/
```

五. 归并排序

> 归并排序把一系列排好序的子序列合并成一个大的完整有序序列。我们需要两个排好序的子数组，然后通过比较数据大小，从最小的数据开始插入，最后合并得到第三个数组。然而，在实际情况中，归并排序还有一些问题，我们需要更大的空间来合并存储两个子数组。

```js
var array = [3,5,7,1,4,56,12,78,25,0,9,8,42,37];
console.log(array);
console.log("-----------------------");
var len = array.length;
sort(0,len);
console.log(array);
function sort(begin,end) {
    if (end - begin < 2) {
        return;
    }
    let mid = (begin + end) >> 1;
    sort(begin, mid);
    sort(mid, end);
    merge(begin,mid,end);
}
function merge(begin,mid,end) {
    let li = 0,le = mid - begin;
    let ri = mid,re = end;
    let ai = begin;
    let leftArray = [];
    for (let i = li;i<le;i++) {
        leftArray[i] = array[begin + i];
    }
    while(li < le){
        if(ri < re && array[ri] < leftArray[li]){
            array[ai++] = array[ri++];
        }else{
            array[ai++] = leftArray[li++];
        }
    }
}
/*
[3, 5, 7, 1, 4, 56, 12, 78, 25, 0, 9, 8, 42, 37]
-----------------------
[0, 1, 3, 4, 5, 7, 8, 9, 12, 25, 37, 42, 56, 78]
*/
```

六. 快速排序

> 快速排序是处理大数据集最快的排序算法之一。它是一种分而治之的算法，通过递归的方法将数据依次分解为包含较小元素和较大元素的不同子序列。该算法不断重复这个步骤直到所有数据都是有序的。
这个算法首先要在列表中选择一个元素作为基准值(pivot)。数据排序围绕基准值进行，将列表中小于基准值的元素移到数组的底部，将大于基准值的元素移到数组的顶部。

```js
function quickSort(arr, i, j) {
  if(i < j) {
    let left = i;
    let right = j;
    let pivot = arr[left];
    while(i < j) {
      while(arr[j] >= pivot && i < j) {  // 从后往前找比基准小的数
        j--;
      }
      if(i < j) {
        arr[i++] = arr[j];
      }
      while(arr[i] <= pivot && i < j) {  // 从前往后找比基准大的数
        i++;
      }
      if(i < j) {
        arr[j--] = arr[i];
      }
    }
    arr[i] = pivot;
    quickSort(arr, left, i-1);
    quickSort(arr, i+1, right);
    return arr;
  }
}

let arr = [2, 10, 4, 1, 0, 9, 5 ,2];
console.log(quickSort(arr, 0 , arr.length-1));
/*
[0, 1, 2, 2, 4, 5, 9, 10]
*/
```


[参与互动](https://github.com/yisainan/web-interview/issues/551)

</details>

<b><details><summary>4. 正则表达式，验证手机号码，验证规则：11 位数字，以 1 位开头</summary></b>

参考答案：

``` js
checkphonenumber(number) {
    if (number == null || number.length != 11) {
        return false
    } else {
        // 移动号段正则表达式
        var pat1 = '^((13[4-9])|(147)|(15[0-2,7-9])|(178)|(18[2-4,7-8]))\\d{8}|(1705)\\d{7}$';
        // 联通号段正则表达式
        var pat2 = '^((13[0-2])|(145)|(15[5-6])|(176)|(18[5,6]))\\d{8}|(1709)\\d{7}$';
        // 电信号段正则表达式
        var pat3 = '^((133)|(153)|(177)|(18[0,1,9])|(149))\\d{8}$';
        // 虚拟运营商正则表达式
        var pat4 = '^((170))\\d{8}|(1718)|(1719)\\d{7}$';
        if (!part1.test(number)) {
            return false
        }
        if (!part2.test(number)) {
            return false
        }
        if (!part3.test(number)) {
            return false
        }
        if (!part4.test(number)) {
            return false
        }
    }
    return true
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/552)

</details>

<b><details><summary>5. 请给 Array 本地对象增加一个原型方法，用于删除数组中重复的条目并按升序排序，返回值是被删除条目的新数组</summary></b>

参考答案：

``` js
Array.prototype.distinct = function() {
    var ret = [];
    for (var i = 0; i < this.length; i++) {
        for (var j = i + 1; j < this.length;) {
            if (this[i] === this[j]) {
                ret.push(this.splice(j, 1)[0]);
            } else {
                j++;
            }
        }
    }
    return ret;
};
let arr = ["a", "b", "c", "d", "b", "a", "e"];
console.log(arr.distinct()); // ['a', 'b']
console.log(arr); // ['a', 'b', 'c', 'd', 'e']
```

[参与互动](https://github.com/yisainan/web-interview/issues/553)

</details>

<b><details><summary>6. 为字符串扩展一个 rewrite 函数，接收一个正则 pattern 和一个字符串 result, 如果该字符串符合 pattern， 则以 result 对结果进行转义输出。</summary></b>

参考答案：

``` js
"/foo".rewrite(/^\/foo/, "/bar");
"u1234".rewrite(/^\/u(\d+)/, "/user/$1");
"/i".rewrite(/^\o/, "/ooo");
```

[参与互动](https://github.com/yisainan/web-interview/issues/554)

</details>

<b><details><summary>7. 实现一个 js 对象序列化函数，将 js 对象序列化为可反序列化的代码，要求 1. 尽量和 json 兼容，2. 支持不可序列化的值，如 undefined/NaN/Infinify-Infinity，3. 支持特殊对象，如正则、Date 等</summary></b>

参考答案：

``` js
serialize({});
serialize({
    a: "b"
});
serialize({
    a: 0 / 0
});
serialize({
    a: /foo/
});
```

[参与互动](https://github.com/yisainan/web-interview/issues/555)

</details>

<b><details><summary>8. 设计一道 JavaScript 的 range 算法如下：</summary></b>

range(1, 10, 3) 返回 [1, 4, 7, 10]; 
range('A', 'F', 2) 返回 ['A', 'C', 'E']; 
请使用 JavaScript 语言实现该功能（可以使用 ES6）

参考答案：

``` js
function range() {
    var args = [].slice.call(arguments); // 相当于Array.slice.call(arguments)，目的是将arguments对象的数组提出来转化为数组，arguments本身并不是数组而是对象
    var str = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'];
    var result = [];
    if (args.length > 2) {
        if (typeof args[0] === 'number') { // 如果是number型数据
            for (var i = args[0]; i <= args[1]; i = i + args[2]) {
                result.push(i);
            }
        } else {
            for (var i = str.indexOf(args[0]); i <= str.indexOf(args[1]); i = i + args[2]) {
                result.push(str[i]);
            }
        }
    }
    return result;
}

range(1, 10, 3); //  [1, 4, 7, 10]
// range('A', 'F', 2); // ['A', 'C', 'E']
```

[参与互动](https://github.com/yisainan/web-interview/issues/556)

</details>

<b><details><summary>9. 头条的视频网站上支持了弹幕，假设一个视频有很多弹幕，弹幕的数据是一个数组，格式定义如下：</summary></b>

``` 

[
    {
        time: Number,
        content: String
    },
    {
        time: Number,
        content: String
    }...
]
(其中 time 表示时间，content表示弹幕内容)，那么如何快速定位到某个时间点的弹幕，请编码实现（不使用数组的 sort 方法）

```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/557)

</details>

<b><details><summary>10. 请写出以下代码的执行结果</summary></b>

``` 
(function() {
    fn();
    var fn = function() {
        alert(1);
    }
    fn();
    function fn() {
        alert(2)
    }
})()
```

参考答案：

第一次弹出2，第二个弹出1

``` 
// 变量提升之后的代码：
(function() {
    function fn() {
        alert(2)
    }
    var fn;
    fn();
    fn = function() {
        alert(1);
    }
    fn();
})()
```

[参与互动](https://github.com/yisainan/web-interview/issues/558)

</details>

<b><details><summary>11. 请说明以下各种情况的执行结果，并注明产生对应结果的理由</summary></b>

``` 
function doSomething() {
    alert(this);
}

a) element.onclick = doSomething, 点击 element 元素后
b) element.onclick = function() doSomething(){}, 点击 element 元素后
c) 直接执行 doSomething()
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/559)

</details>

<b><details><summary>12. 请写出以下代码的执行结果</summary></b>

例1：

```js
var obj = new Object();
var events = {m1: 'clicked', m2: 'changed'};

for (var e in events) {
    (function() {
        var aValue = e;
        obj[e] = function() {
            // var aValue = e;
            console.log(events[aValue]);
        };
    }());
};

console.log(obj.m1 === obj.m2); // false

obj.m1(); // clicked
obj.m2(); // changed
```

例2：

```js
var obj = new Object();
var events = {m1: 'clicked', m2: 'changed'};

for (var e in events) {
    (function() {
        // var aValue = e;
        obj[e] = function() {
            var aValue = e;
            console.log(events[aValue]);
        };
    }());
};

console.log(obj.m1 === obj.m2); // false

obj.m1(); // changed
obj.m2(); // changed
```

参考答案：

例1：false clicked changed
例2：false changed changed

解析：

以上两个例子中，除了var aValue = e;这一句位置不同：例1位于外层匿名函数中、例2位于内层匿名函数中，其他部分完全相同。为什么结果不同？

例1：for 循环进行的过程中，就把当时的 e 像拍照一样封存在了aValue变量里（注意，这里每一次循环都产生了一个新的闭包，所以循环了几次就有几个aValue同时存在，本例是2个，它们的值分别是'm1' 和 'm2'），当你调用obj.m1() 时，取的是闭包中的aValue，而不是现在的 e 了。

例2：内层函数obj.m1和obj.m2是在循环结束后才执行的，此时循环变量e的值为'm2'（注意 e 是 for 循环的循环变量，而当你调用 obj.m1() 和 obj.m2()的时候，for循环早已结束了，因此它的循环变量 e 已经永远地停留在了 'm2'），因此obj.m1和obj.m2中的局部变量aValue的值只能是'm2'。

[参与互动](https://github.com/yisainan/web-interview/issues/560)

</details>

<b><details><summary>13. 请写出类 Son 继承类 Father</summary></b>

function Father() {}
function Son() {}

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/561)

</details>

<b><details><summary>14. 请用 JS 写出一个遍历 DOM 节点树的方法</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/562)

</details>

<b><details><summary>15. 尝试实现注释部分的 JavaScript 代码， 可在其他任何地方添加更多代码。</summary></b>

``` 
var Obj = function(msg) {
    this.msg = msg;
    this.shout = function () {
        alert(this.msg)
    }
    this.waitAndShout = function() {
        // 隔五秒钟后执行上面的 shout 方法
    }
}
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/565)

</details>

<b><details><summary>16. 请编写一个 JavaScript 函数 parseQuerySting, 它的用途是把 URL 参数解析为一个对象，如</summary></b>

``` 
var url = "http://www.58.com/index.aspx?key0=0&key1=1&key2=2..."
var obj = parseQuerySting(url);
alert(obj.key0) // 输出 0
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/566)

</details>

<b><details><summary>17. 请给 Array 本地对象添加一个原型方法，它用于删除数组条目中重复的条目（可能有多个重复），返回值是一个包含被删除的重复条目的新数组</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/567)

</details>

<b><details><summary>18. 我们把一个数字倒着读和原数字相同的数字称之为对称数，例如（1, 121, 88, 8998）, 不考虑性能，请找出 1 - 10000 之间的对称数，要求用 JS 实现</summary></b>

参考答案：

```js
function findSymmetryNum(s, o) {
	var arr = [];
	for (var i = s; i <= o; i++) {
		var str = '' + i,
		sl = str.length,
		middle = 0,
		flag = true;
        // 字符串分割成两半，记录中间数值middle
		if (sl % 2 === 0) {
			middle = sl / 2;
		} else {
			middle = (sl - 1) / 2;
		}
        // 判断middle左右的数是否对称
		for (var m = 0; m < middle; m++) {
			if (str.substr(0 + m, 1) !== str.substr( - 1 - m, 1)) {
				flag = false;
			}
		}
		flag && arr.push(i);
	}
	console.log(arr);
	return arr;
}
findSymmetryNum(1, 10000);
```

[参与互动](https://github.com/yisainan/web-interview/issues/568)

</details>

<b><details><summary>19. 以下代码输出多少</summary></b>

``` js
var name = "world";
(function() {
    if (typeof name === "undefined") {
        var name = "jack";
        console.log("Hi!" + name);
    } else {
        console.log("Hello," + name)
    }
})()

==
>
Hi!jack

var name = "world";
(function(name) {
    if (typeof name === "undefined") {
        var name = "jack";
        console.log("Hi!" + name);
    } else {
        console.log("Hello," + name)
    }
})(name)

==
>
Hello, world
```

[参与互动](https://github.com/yisainan/web-interview/issues/569)

</details>

<b><details><summary>20. js 数组拍平(数组扁平化)的六种方式</summary></b>

参考答案：

1. 数组拍平也称数组扁平化，就是将数组里面的数组打开，最后合并为一个数组

2. 实现

``` js
var arr = [1, 2, [3, 4, 5, [6, 7, 8], 9], 10, [11, 12]];
```

a：递归实现

``` js
function fn(arr) {
    let arr1 = [];
    arr.forEach(val => {
        if (val instanceof Array) {
            arr1 = arr1.concat(fn(val));
        } else {
            arr1.push(val);
        }
    });
    return arr1;
}
```

b：reduce 实现

``` js
function fn(arr) {
    return arr.reduce((prev, cur) => {
        return prev.concat(Array.isArray(cur) ? fn(cur) : cur);
    }, []);
}
```

c:flat

参数为层数(默认一层)

``` js
arr.flat(Infinity);
```

d：扩展运算符

``` js
function fn(arr) {
    let arr1 = [];
    let bStop = true;
    arr.forEach(val => {
        if (Array.isArray(val)) {
            arr1.push(...val);
            bStop = false;
        } else {
            arr1.push(val);
        }
    });
    if (bStop) {
        return arr1;
    }
    return fn(arr1);
}
```

e：toString

``` js
let arr1 = arr
    .toString()
    .split(",")
    .map(val => {
        return parseInt(val);
    });
console.log(arr1);
```

f：apply

``` js
function flatten(arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat.apply([], arr);
    }
    return arr;
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/570)

</details>

<b><details><summary>21. 如何解决数组塌陷问题</summary></b>

什么叫数组塌陷？

一个数组在进行删除数据单元操作的时候，删除掉这个单元之后，后面的数据单元会自动的补充的这个位置上来，造成数组长度的减少，这种情况被称之为数组塌陷。

例：循环删除数组中的数据，每循环一次，删除一个数据单元

``` js
var arr = [0,1,2,3,4,5,6,7,8,9];
for (var i = 0; i < arr.length; i++) {
    arr.splice(i, 1);
}
console.log(arr) // [1, 3, 5, 7, 9]
```

那么 现在的输出是 [1, 3, 5, 7, 9] 还剩 5 个 单元 ，也就是还有一半没有删除。

因为：我们使用for 循环遍历 arr, 当i = 0的时候, 我们删除了位置为 0 的元素,此时位置为 1 的元素接替了位置 0 , 但同时 i 也累加了, 下次执行删除操作的时候 i 变为 1,再次执行删除操作,其实是删除了现在位置为 1 的元素, 中间跳过了, 所以最后的结果只删除了一半。

如何解决数组塌陷问题呢？

参考答案：

``` js
// 方法1 使用i--
for (var i = 0; i < arr.length; i++) {
    arr.splice(i, 1);
    i--;
}
console.log(arr); // []

// 方法2 从数组的末尾一项开始遍历
for (var i = arr.length - 1; i >= 0; i--) {
    arr.splice(i, 1);
}
console.log(arr); // []
```

[参与互动](https://github.com/yisainan/web-interview/issues/571)

</details>

<b><details><summary>22. 计算打印结果</summary></b>

参考答案：

``` js
function fun(n, o) {
    console.log(o);
    return {
        fun: function(m) {
            return fun(m, n);
        }
    };
}
// var a = fun(0);
// a.fun(1);
// a.fun(2);
// a.fun(3);

// 打印
// undefined 0 0 0

// var b = fun(0).fun(1).fun(2).fun(3)
// 打印 undefined 0 1 2

var c = fun(0).fun(1);
c.fun(2);
c.fun(3);
// 打印
// undefined 0 1 1
```

[参与互动](https://github.com/yisainan/web-interview/issues/572)

</details>

<b><details><summary>23. 编写一个数组去重的方法</summary></b>

参考答案：

``` js
var arr = [1, 2, 3, 3, 4, 4, 5, 5, 6, 1, 9, 3, 25, 4];

function deRepeat() {
    var newArr = [];
    var obj = {};
    var index = 0;
    var l = arr.length;
    for (var i = 0; i < l; i++) {
        if (obj[arr[i]] == undefined) {
            obj[arr[i]] = 1;
            newArr[index++] = arr[i];
        } else if (obj[arr[i]] == 1) continue;
    }
    return newArr;
}
var newArr2 = deRepeat(arr);
alert(newArr2); //输出1,2,3,4,5,6,9,25
```

[参与互动](https://github.com/yisainan/web-interview/issues/573)

</details>

<b><details><summary>24. 已知 id 的 input 输入框，希望获取这个输入框的输入值，怎么做？（不使用第三方框架）</summary></b>

参考答案：

``` js
document.getElementById("id").value;
```

[参与互动](https://github.com/yisainan/web-interview/issues/574)

</details>

<b><details><summary>25. 获取到页面中所有的 checkbox 怎么做？（不使用第三方框架）</summary></b>

参考答案：

``` js
var domList = document.getElementsByTagName("input");
var ckList = []; // 返回的所有的 checkbox
var len = domList.length;
for (var i = 0; i < len; i++) {
    if (domList[i].type == "checkbox") {
        ckList.push(domList[i]);
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/575)

</details>

<b><details><summary>26. 设置一个已知 id 的 div 的 html 内容为 xxxx，字体颜色设置为黑色（不使用第三方框架）</summary></b>

参考答案：

``` js
var dom = document.getElementById("id");
dom.innerHTML = "xxxx";
dom.style.color = "#000"; // 'black'
```

[参与互动](https://github.com/yisainan/web-interview/issues/576)

</details>

<b><details><summary>27. JavaScript 中的相等性判断</summary></b>

参考答案：

``` js
2 == true[] == false[] == ![]
```

解析：[参考](https://www.cnblogs.com/youyoui/p/8385542.html)

[参与互动](https://github.com/yisainan/web-interview/issues/577)

</details>

<b><details><summary>28. 已知有字符串 foo="get-element-by-id", 写一个 function 将其转化为驼峰表示法"getElementById"</summary></b>

参考答案：

``` js
var string = "get-element-by-id";

function combo(msg) {
    var arr = msg.split("-"); //split("-")以-为分隔符截取字符串，返回数组
    for (var i = 1; i < arr.length; i++) {
        arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
    }
    msg = arr.join(""); //join()返回字符串
    return msg;
}
console.log(combo(string));
```

[参与互动](https://github.com/yisainan/web-interview/issues/578)

</details>

<b><details><summary>29. 看下面代码，给出输出结果</summary></b>

``` js
for (var i = 1; i <= 3; i++) {
    console.log(i);
}
// 1 2 3
```

但是

``` js
for (var i = 1; i <= 3; i++) {
    setTimeout(() => {
        // setTimout在for里面是异步执行的，在延迟输出的时候，i的值已经是4了
        console.log(i);
    }, 0);
}
// 4 4 4
```

如何输出 1 2 3

参考答案：

1. 立即执行函数

``` js
for (var i = 1; i <= 3; i++) {
    setTimeout(
        (i => {
            console.log(i);
        })(i),
        0
    );
}
```

2. 闭包

``` js
for (var i = 1; i <= 3; i++) {
    setTimeout(
        (() => {
            var j = i;
            return function() {
                console.log(j);
            };
        })(),
        0
    );
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/579)

</details>

<b><details><summary>30. JS 字符串使用堆栈处理 "(a, b, (c, d), f, g)"</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/580)

</details>

<b><details><summary>31. 二维数组操作</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/581)

</details>

<b><details><summary>32. 用最简单的方式，求一个数组中最大的元素，例如 arr=[5, 7, 9, 42, 18, 29]</summary></b>

参考答案：

``` js
var a = [1, 2, 3, 5];
alert(Math.max.apply(null, a)); //最大值
alert(Math.min.apply(null, a)); //最小值
```

[参与互动](https://github.com/yisainan/web-interview/issues/582)

</details>

<b><details><summary>33. 写一个 function，清除字符串前后的空格（兼容所有的浏览器）</summary></b>

参考答案：

``` js
//重写trim方法
if (!String.prototype.trim) {
    String.prototype.trim = function() {
        return this.replace(/^\s+/, "").replace(/\s+$/, "");
    };
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/583)

</details>

<b><details><summary>34. 运算符面试题</summary></b>

参考答案：

``` js
var a = 10,
    b = 20,
    c = 30;
++a;
a++;
e = ++a + ++b + c++ + a++;
console.log(e); // 77
```

[参与互动](https://github.com/yisainan/web-interview/issues/584)

</details>

<b><details><summary>35. this 面试题</summary></b>

参考答案：

``` 
 this指向了谁？
 看函数在执行的时候是如何调用的，
 1 如果这个函数是用普通函数调用模式来进行调用，它内部的this指向了window
 2如果一个函数在调用的时候是通过对象方法模式来进行调用，则它内部的this就是我们的对象
 3 如果一个函数在调用的时候通过构造函数模式调用，则它内部的this指向了生成的实例
 4 如果这个函数是通过方法借用模式调用，则这个函数内部的this就是我们手动指定this。
```

``` js
//第1题
function Fn() {
    console.log(this);
}
Fn(); //window 普通函数调用模式
new Fn(); //{}  构造函数调用模式
Fn.apply(Fn); // Fn的函数体   方法借用模式

//第2题
var o = {
    f: function() {
        console.log(this);
    },
    2: function() {
        console.log(this);
        console.log(this.__proto__ === o[2].prototype);
    }
};
o.f(); //o   对象调用模式
o[2](); //o  对象调用模式
new o[2](); //存疑，存在着优先级的问题 {}  通过构造函数模式进行调用
o.f.call([1, 2]); //[1,2]   call方法进行方法借用。
o[2].call([1, 2, 3, 4]); // [1,2,3,4]  call方法进行方法借用

//第3题
var name = "out";
var obj = {
    name: "in",
    prop: {
        name: "inside",
        getName: function() {
            return this.name;
        }
    }
};

console.log(obj.prop.getName()); //对象调用模式来进行调用  obj.prop.name  'inside'
var test = obj.prop.getName; // 把test这个变量指向了obj.prop.getName所在的内存地址。
console.log(test()); //普通函数模式来进行调用  window 'out'
console.log(obj.prop.getName.apply(window)); //方法借用模式  'out'
console.log(obj.prop.getName.apply(this)); //方法借用模式  'out'
console.log(this === window); //true

//第4题
var length = 10;

function fn() {
    console.log(this.length);
}
var obj = {
    length: 5,
    method: function(f) {
        console.log(this);
        f(); // f在调用的时候是什么调用模式？普通函数调用模式  window.length  10
        arguments[0](); // 通过什么模式来进行调用的。执行之前有[]和.就是对象调用模式。
        //arguments是一个类数组，也就是一个对象，就是通过arguments来进行调用的
        //arguments.length实参的数量。实参长度是1
        //通过arguments对象进行调用，因此函数内部的this是  arguments
        // 如果一个函数在调用的时候它前面有call和apply那么就肯定是方法借用模式调用
        arguments[0].call(this);
        // 调用method方法是通过obj.method 因此在这里的this就是 obj
        //通过call方法把fn内的this指向了obj
        // 输出obj.length  5
    }
};
obj.method(fn);

//第5题
function Foo() {
    getName = function() {
        console.log(1);
    };
    return this;
}
Foo.getName = function() {
    console.log(2);
};
Foo.prototype.getName = function() {
    console.log(3);
};
var getName = function() {
    console.log(4);
};

function getName() {
    console.log(5);
}
//请写出以下输出结果：
Foo.getName(); //2
getName(); //4
Foo().getName(); //1
getName(); //1
new Foo.getName(); //2
new Foo().getName(); //3
new new Foo().getName(); //3
// new Foo()创建了一个构造函数，然后这个函数再去访问getName这个函数，
//对它进行调用
/*console.log(new Foo().getName)*/
/*var o = new new Foo().getName(); //
    console.log(o.__proto__===Foo.prototype.getName.prototype)*/
//用new Foo创建出来了一个实例，然后这个实例去访问 (new Foo().getName)

/*console.log(new new Foo().getName())

    console.log(new Foo().getName())*/

/*function Foo() {
        getName = function () {
            console.log(1);
        };
        return this;
    }
    var getName;
    Foo.getName = function () {
        console.log(2);
    };
    Foo.prototype.getName = function () {
        console.log(3);
    };
    getName = function () {
        console.log(4);
    };
    //请写出以下输出结果：
    Foo.getName();// 2
    getName();//4
/!*    Foo().getName();//!*!/
    window.getName()//1
    getName();//1
  /!*  var o = new Foo.getName();//2
    console.log(o);// {}
    console.log(o.__proto__===Foo.getName.prototype)//true*!/
    new Foo.getName();// 2
    new Foo().getName();//
    new new Foo().getName();*/

//第6题
var obj = {
    fn: function() {
        console.log(this);
    }
};
obj.fn(); //obj
var f = obj.fn;
f(); //window
console.log(f === obj.fn); // true

// f和obj.fn是同一个函数，但是他们在调用的时候使用的函数调用模式不同，因此，它们内部的this指向也就不同。

// #7题
var arr = [
    function() {
        console.log(this);
    }
];
arr[0](); //数组本身
//数组也是一个复杂数据类型，也是一个对象，那用数组去调用函数，使用的模式就是对象方法调用模式。
function f() {
    console.log(this);
}

function fn() {
    console.log(arguments); // 类数组，也是就一个对象    [0:function f(){}]
    console.log(this); // window
    arguments[0]();
    console.log(arguments[0]); //内部的this就是arguments
    // 通过arguments对f这个方法进行调用，使用的是对象方法调用模式。
}
fn(f);

// #8题
function SuperClass() {
    this.name = "women";
    this.bra = ["a", "b"];
}

SuperClass.prototype.sayWhat = function() {
    console.log("hello");
};

function SubClass() {
    this.subname = "you sister";
    SuperClass.call(this);
}

var sub = new SubClass();
console.log(sub.sayWhat());
```

[参与互动](https://github.com/yisainan/web-interview/issues/585)

</details>

<b><details><summary>36. 实现一个 new 操作符</summary></b>

参考答案：

``` js
function New(func) {
    var res = {};
    if (func.prototype !== null) {
        res.__proto__ = func.prototype;
    }
    var ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
    if ((typeof ret === "object" || typeof ret === "function") && ret !== null) {
        return;
        ret;
    }
    return;
    res;
}
var obj = New(A, 1, 2);
// equals to
var obj = new A(1, 2);
```

[参与互动](https://github.com/yisainan/web-interview/issues/586)

</details>

<b><details><summary>37. 实现一个 call 或 apply</summary></b>

参考答案：

1. call

``` js
Function.prototype.call2 = function(context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for (var i = 1, len = arguments.length; i < len; i++) {
        args.push("arguments[" + i + "]");
    }

    var result = eval("context.fn(" + args + ")");

    delete context.fn;
    return result;
};
```

2. apply

``` js
Function.prototype.apply2 = function(context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    } else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push("arr[" + i + "]");
        }
        result = eval("context.fn(" + args + ")");
    }

    delete context.fn;
    return result;
};
```

[参与互动](https://github.com/yisainan/web-interview/issues/587)

</details>

<b><details><summary>38. 实现一个 Function. bind</summary></b>

参考答案：

``` js
Function.prototype.bind2 = function(context) {
    if (typeof this !== "function") {
        throw new Error(
            "Function.prototype.bind - what is trying to be bound is not callable"
        );
    }
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);
    var fNOP = function() {};
    var fbound = function() {
        self.apply(
            this instanceof self ? this : context,
            args.concat(Array.prototype.slice.call(arguments))
        );
    };
    fNOP.prototype = this.prototype;
    fbound.prototype = new fNOP();
    return fbound;
};
```

[参与互动](https://github.com/yisainan/web-interview/issues/588)

</details>

<b><details><summary>39. 实现一个继承</summary></b>

参考答案：

``` js
function Parent(name) {
    this.name = name;
}

Parent.prototype.sayName = function() {
    console.log("parent name:", this.name);
};

function Child(name, parentName) {
    Parent.call(this, parentName);
    this.name = name;
}

function create(proto) {
    function F() {}
    F.prototype = proto;
    return new F();
}
Child.prototype = create(Parent.prototype);
Child.prototype.sayName = function() {
    console.log("child name:", this.name);
};

Child.prototype.constructor = Child;
var parent = new Parent("汪某");
parent.sayName(); // parent name: 汪某
var child = new Child("son", "汪某");
```

[参与互动](https://github.com/yisainan/web-interview/issues/589)

</details>

<b><details><summary>40. 手写一个 Promise(中高级必考)</summary></b>

参考答案：

``` js
function myPromise(constructor) {
    let self = this;
    self.status = "pending";
    //定义状态改变前的初始状态
    self.value = undefined;
    //定义状态为resolved的时候的状态
    self.reason = undefined;
    //定义状态为rejected的时候的状态
    function resolve(value) {
        //两个==="pending"，保证了状态的改变是不可逆的
        if (self.status === "pending") {
            self.value = value;
            self.status = "resolved";
        }
    }

    function reject(reason) {
        //两个==="pending"，保证了状态的改变是不可逆的
        if (self.status === "pending") {
            self.reason = reason;
            self.status = "rejected";
        }
    }
    //捕获构造异常
    try {
        constructor(resolve, reject);
    } catch (e) {
        reject(e);
    }
}

//同时，需要在 myPromise的原型上定义链式调用的 then方法：
myPromise.prototype.then = function(onFullfilled, onRejected) {
    let self = this;
    switch (self.status) {
        case "resolved":
            onFullfilled(self.value);
            break;
        case "rejected":
            onRejected(self.reason);
            break;
        default:
    }
};

//测试一下：
var p = new myPromise(function(resolve, reject) {
    resolve(1);
});
p.then(function(x) {
    console.log(x);
});
```

[参与互动](https://github.com/yisainan/web-interview/issues/590)

</details>

<b><details><summary>41. 手写防抖(Debouncing)和节流(Throttling)</summary></b>

参考答案：

``` js
// 防抖函数
function debounce(fn, wait) {
    let timer;
    return function() {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            fn.apply(this, arguments);
        }, wait);
    };
}

// （参考博客https://segmentfault.com/a/1190000018428170）
function debounce(fn, delay) {
    let timer = null //借助闭包
    return function() {
        if (timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(fn, delay) // 简化写法
    }
}
```

``` js
// 节流函数 通过时间戳差值是否大于指定间隔时间来做判定
function throttle(fn, wait) {
    let prev = new Date();
    return function() {
        const args = arguments;
        const now = new Date();
        if (now - prev > wait) {
            fn.apply(this, args);
            prev = new Date();
        }
    };
}

// 通过setTimeout的返回的标记当做判断条件实现（参考博客https://segmentfault.com/a/1190000018428170）
function throttle(fn, delay) {
    let valid = true
    return function() {
        if (!valid) {
            //休息时间 暂不接客
            return false
        }
        // 工作时间，执行函数并且在间隔期内把状态位设为无效
        valid = false
        setTimeout(() => {
            fn()
            valid = true;
        }, delay)
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/591)

</details>

<b><details><summary>42. 手写一个 JS 深拷贝</summary></b>

参考答案：

``` js
function deepCopy(obj) {
    //判断是否是简单数据类型，
    if (typeof obj == "object") {
        //复杂数据类型
        var result = obj.constructor == Array ? [] : {};
        for (let i in obj) {
            result[i] = typeof obj[i] == "object" ? deepCopy(obj[i]) : obj[i];
        }
    } else {
        //简单数据类型 直接 == 赋值
        var result = obj;
    }
    return result;
}
```

``` js
let o1 = {
    a: {
        b: 1
    }
};
let o2 = JSON.parse(JSON.stringify(o1));
```

另一种方法

``` js
function deepCopy(s) {
    const d = {};
    for (let k in s) {
        if (typeof s[k] == "object") {
            d[k] = deepCopy(s[k]);
        } else {
            d[k] = s[k];
        }
    }

    return d;
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/592)

</details>

<b><details><summary>43. 看下面代码，给出输出结果(考察闭包及++运算符)</summary></b>

参考答案：

``` js
function Foo() {
    var i = 0;
    return function() {
        console.log(i++);
    };
}
var f1 = Foo(),
    f2 = Foo();

f1(); // 0
f1(); // 1
f2(); // 0
```

``` js
function fn() {
    var a = 1;
    return function() {
        a++;
        console.log(a);
    };
}
var b = fn();
console.log(b());
// 2
```

``` js
function fn() {
    var a = 1;
    return function() {
        console.log(a++);
    };
}
var b = fn();
console.log(b());
// 1
```

[参与互动](https://github.com/yisainan/web-interview/issues/593)

</details>

<b><details><summary>44. 看下面代码，给出输出结果(考察时间戳)</summary></b>

参考答案：

``` js
//总结：第一个setTimeout，时间间隔<1000的话，输出1000多，>1000的话，输出间隔值多
//     第二个setTimeout，是1000+时间间隔
var dateNum = new Date();
setTimeout(function() {
    console.log(new Date() - dateNum);
}, 1200); //1200多
while (new Date() - dateNum < 1000) {
    var a = 1;
}
setTimeout(function() {
    console.log(new Date() - dateNum);
}, 1500); // 2500左右
```

[参与互动](https://github.com/yisainan/web-interview/issues/594)

</details>

<b><details><summary>45. 编写一个元素拖拽的插件</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/595)

</details>

<b><details><summary>46. 什么是代理和通知，写一下他们基本的实现方法</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/596)

</details>

<b><details><summary>47. 看题写结果</summary></b>

``` js
var output = function(i) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
};

for (var i = 0; i < 5; i++) {
    output(i); // 这里传过去的 i 值被复制了
}

console.log(i);
```

参考答案：

5
0
1
2
3
4

解析：[参考](https://www.cnblogs.com/adouwt/p/6481479.html)

[参与互动](https://github.com/yisainan/web-interview/issues/597)

</details>

<b><details><summary>48. 告诉我参考答案是多少</summary></b>

``` js
(function(x) {
    delete x;
    alert(x);
})(1 + 5);
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/598)

</details>

<b><details><summary>49. 写一个通用的事件侦听器函数</summary></b>

参考答案：

``` js
// event(事件)工具集，来源：https://github.com/markyun
markyun.Event = {
    // 页面加载完成后
    readyEvent: function(fn) {
        if (fn == null) {
            fn = document;
        }
        var oldonload = window.onload;
        if (typeof window.onload != "function") {
            window.onload = fn;
        } else {
            window.onload = function() {
                oldonload();
                fn();
            };
        }
    },
    // 视能力分别使用dom0||dom2||IE方式 来绑定事件
    // 参数： 操作的元素,事件名称 ,事件处理程序
    addEvent: function(element, type, handler) {
        if (element.addEventListener) {
            //事件类型、需要执行的函数、是否捕捉
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, function() {
                handler.call(element);
            });
        } else {
            element["on" + type] = handler;
        }
    },
    // 移除事件
    removeEvent: function(element, type, handler) {
        if (element.removeEnentListener) {
            element.removeEnentListener(type, handler, false);
        } else if (element.datachEvent) {
            element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
        }
    },
    // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
    stopPropagation: function(ev) {
        if (ev.stopPropagation) {
            ev.stopPropagation();
        } else {
            ev.cancelBubble = true;
        }
    },
    // 取消事件的默认行为
    preventDefault: function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    // 获取事件目标
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    // 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
    getEvent: function(e) {
        var ev = e || window.event;
        if (!ev) {
            var c = this.getEvent.caller;
            while (c) {
                ev = c.arguments[0];
                if (ev && Event == ev.constructor) {
                    break;
                }
                c = c.caller;
            }
        }
        return ev;
    }
};
```

[参与互动](https://github.com/yisainan/web-interview/issues/599)

</details>

<b><details><summary>50. 谈一下 JS 中的递归函数，并且用递归简单实现阶乘</summary></b>

参考答案：递归即是程序在执行过程中不断调用自身的编程技巧，当然也必须要有一个明确的结束条件，不然就会陷入死循环。

[参与互动](https://github.com/yisainan/web-interview/issues/600)

</details>

<b><details><summary>51. 请用正则表达式写一个简单的邮箱验证</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/601)

</details>

<b><details><summary>52. 完成 foo()函数的内容，要求能够弹出对话框提示当前选中的是第几个单选框</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/602)

</details>

<b><details><summary>53. 完成函数 showImg()，要求能够动态根据下拉列表的选项变化，更新图片的显示</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/630)

</details>

<b><details><summary>54. 截取字符串 abcdefg 中的 efg</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/631)

</details>

<b><details><summary>55. 在 Javascript 中什么是伪数组？如何将伪数组转化为标准数组？</summary></b>

参考答案：

伪数组（类数组）：无法直接调用数组方法或期望 length 属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。典型的是函数的 argument 参数，还有像调用 getElementsByTagName, document. childNodes 之类的, 它们都返回 NodeList 对象都属于伪数组。可以使用 Array. prototype. slice. call(fakeArray)将数组转化为真正的 Array 对象。

假设我们要给每个 log 方法添加一个"(app)"前缀，比如'hello world!' ->'(app)hello world!'。方法如下：

``` js
function log() {
    var args = Array.prototype.slice.call(arguments); //为了使用unshift数组方法，将argument转化为真正的数组
    args.unshift("(app)");
    console.log.apply(console, args);
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/632)

</details>

<b><details><summary>56. 判断一个字符串中出现次数最多的字符，统计这个次数</summary></b>

参考答案：

``` js
var str = "asdfssaaasasasasaa";
var json = {};
for (var i = 0; i < str.length; i++) {
    if (!json[str.charAt(i)]) {
        json[str.charAt(i)] = 1;
    } else {
        json[str.charAt(i)]++;
    }
}
var iMax = 0;
var iIndex = "";
for (var i in json) {
    if (json[i] > iMax) {
        iMax = json[i];
        iIndex = i;
    }
}
alert("出现次数最多的是:" + iIndex + "出现" + iMax + "次");
```

[参与互动](https://github.com/yisainan/web-interview/issues/633)

</details>

<b><details><summary>57. 写一个获取非行间样式的函数</summary></b>

参考答案：

``` js
function getStyle(obj, attr, value) {
    if (!value) {
        if (obj.currentStyle) {
            return obj.currentStyle(attr);
        } else {
            obj.getComputedStyle(attr, false);
        }
    } else {
        obj.style[attr] = value;
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/634)

</details>

<b><details><summary>58. 字符串反转，如将 '12345678' 变成 '87654321'</summary></b>

参考答案：

``` js
//思路：先将字符串转换为数组 split()，利用数组的反序函数 reverse()颠倒数组，再利用 jion() 转换为字符串
var str = "12345678";
str = str
    .split("")
    .reverse()
    .join("");
```

[参与互动](https://github.com/yisainan/web-interview/issues/635)

</details>

<b><details><summary>59. 将数字 12345678 转化成 RMB 形式 如： 12, 345, 678</summary></b>

参考答案：

``` js
//个人方法；
//思路：先将数字转为字符， str= str + '' ;
//利用反转函数，每三位字符加一个 ','最后一位不加； re()是自定义的反转函数，最后再反转回去！
for (var i = 1; i <= re(str).length; i++) {
    tmp += re(str)[i - 1];
    if (i % 3 == 0 && i != re(str).length) {
        tmp += ",";
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/636)

</details>

<b><details><summary>60. 生成 5 个不同的随机数</summary></b>

参考答案：

``` js
//思路：5个不同的数，每生成一次就和前面的所有数字相比较，如果有相同的，则放弃当前生成的数字！
var num1 = [];
for (var i = 0; i < 5; i++) {
    num1[i] = Math.floor(Math.random() * 10) + 1; //范围是 [1, 10]
    for (var j = 0; j < i; j++) {
        if (num1[i] == num1[j]) {
            i--;
        }
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/637)

</details>

<b><details><summary>61. 去掉数组中重复的数字</summary></b>

参考答案：

方法一

``` js
//思路：每遍历一次就和之前的所有做比较，不相等则放入新的数组中！
//这里用的原型 个人做法；
Array.prototype.unique = function() {
    var len = this.length,
        newArr = [],
        flag = 1;
    for (var i = 0; i < len; i++, flag = 1) {
        for (var j = 0; j < i; j++) {
            if (this[i] == this[j]) {
                flag = 0; //找到相同的数字后，不执行添加数据
            }
        }
        flag ? newArr.push(this[i]) : "";
    }
    return newArr;
};
```

方法二

``` js
(function(arr) {
    var len = arr.length,
        newArr = [],
        flag;
    for (var i = 0; i < len; i += 1, flag = 1) {
        for (var j = 0; j < i; j++) {
            if (arr[i] == arr[j]) {
                flag = 0;
            }
        }
        flag ? newArr.push(arr[i]) : "";
    }
    alert(newArr);
})([1, 1, 22, 3, 4, 55, 66]);
```

[参与互动](https://github.com/yisainan/web-interview/issues/638)

</details>

<b><details><summary>62. 阶乘函数</summary></b>

参考答案：

``` js
//原型方法
Number.prototype.N = function() {
    var re = 1;
    for (var i = 1; i <= this; i++) {
        re *= i;
    }
    return re;
};
var num = 5;
alert(num.N());
```

[参与互动](https://github.com/yisainan/web-interview/issues/639)

</details>

<b><details><summary>63. 看题做答</summary></b>

参考答案：

``` js
function f1() {
    var tmp = 1;
    this.x = 3;
    console.log(tmp); //A
    console.log(this.x); //B
}
var obj = new f1(); //1
console.log(obj.x); //2
console.log(f1()); //3
```

解析：   
     这道题让我重新认识了对象和函数，首先看代码（1），这里实例话化了 f1 这个类。相当于执行了 f1 函数。所以这个时候 A 会输出 1， 而 B 这个时候的 this 代表的是 实例化的当前对象 obj B 输出 3. 。 代码（2）毋庸置疑会输出 3， 重点 代码（3）首先这里将不再是一个类，它只是一个函数。那么 A 输出 1， B 呢？这里的 this 代表的其实就是 window 对象，那么 this. x 就是一个全局变量 相当于在外部 的一个全局变量。所以 B 输出 3。最后代码由于 f 没有返回值那么一个函数如果没返回值的话，将会返回 underfined ，所以参考答案就是 ： 1， 3， 3， 1， 3， underfined 。

[参与互动](https://github.com/yisainan/web-interview/issues/640)

</details>

<b><details><summary>64. 下面输出多少？</summary></b>

参考答案：

``` js
var o1 = new Object();
var o2 = o1;
o2.name = "CSSer";
console.log(o1.name);
```

解析：

如果不看参考答案，你回答真确了的话，那么说明你对 javascript 的数据类型了解的还是比较清楚了。js 中有两种数据类型，分别是：基本数据类型和引用数据类型（object Array）。对于保存基本类型值的变量，变量是按值访问的，因为我们操作的是变量实际保存的值。对于保存引用类型值的变量，变量是按引用访问的，我们操作的是变量值所引用（指向）的对象。参考答案就清楚了：  CSSer; 

[参与互动](https://github.com/yisainan/web-interview/issues/641)

</details>

<b><details><summary>65. 下面输出多少？</summary></b>

参考答案：

``` js
function changeObjectProperty(o) {
    o.siteUrl = "http://www.csser.com/";
    o = new Object();
    o.siteUrl = "http://www.popcg.com/";
}
var CSSer = new Object();
changeObjectProperty(CSSer);
console.log(CSSer.siteUrl); //
```

解析：

如果 CSSer 参数是按引用传递的，那么结果应该是 `"http://www.popcg.com/"，但实际结果却仍是"http://www.csser.com/"。事实是这样的：在函数内部修改了引用类型值的参数，该参数值的原始引用保持不变。我们可以把参数想象成局部变量，当参数被重写时，这个变量引用的就是一个局部变量，局部变量的生存期仅限于函数执行的过程中，函数执行完毕，局部变量即被销毁以释放内存。` 
    （补充：内部环境可以通过作用域链访问所有的外部环境中的变量对象，但外部环境无法访问内部环境。每个环境都可以向上搜索作用域链，以查询变量和函数名，反之向下则不能。）

[参与互动](https://github.com/yisainan/web-interview/issues/642)

</details>

<b><details><summary>66. 输出多少？</summary></b>

参考答案：

``` js
var a = 6;
setTimeout(function() {
    var a = 666;
    alert(a); // 输出666，
}, 1000);
a = 66;
```

因为 var a = 666; 定义了局部变量 a，并且赋值为 666，根据变量作用域链，
全局变量处在作用域末端，优先访问了局部变量，从而覆盖了全局变量 。

``` js
var a = 6;
setTimeout(function() {
    alert(a); // 输出undefined
    var a = 666;
}, 1000);
a = 66;
```

因为 var a = 666; 定义了局部变量 a，同样覆盖了全局变量，但是在 alert(a); 之前
a 并未赋值，所以输出 undefined。

``` js
var a = 6;
setTimeout(function() {
    alert(a);
    var a = 66;
}, 1000);
a = 666;
alert(a);
// 666, undefined;
```

记住： 异步处理，一切 OK 声明提前

[参与互动](https://github.com/yisainan/web-interview/issues/643)

</details>

<b><details><summary>67. JS 的继承性？</summary></b>

参考答案：

``` js
window.color = "red";
var o = {
    color: "blue"
};

function sayColor() {
    alert(this.color);
}
sayColor(); //red
sayColor.call(this); //red this-window对象
sayColor.call(window); //red
sayColor.call(o); //blue
```

[参与互动](https://github.com/yisainan/web-interview/issues/644)

</details>

<b><details><summary>68. 精度问题: JS 精度不能精确到 0. 1 所以  。。。。同时存在于值和差值中</summary></b>

参考答案：

``` js
var n = 0.3,
    m = 0.2,
    i = 0.2,
    j = 0.1;
alert(n - m == i - j); //false
alert(n - m == 0.1); //false
alert(i - j == 0.1); //true
```

[参与互动](https://github.com/yisainan/web-interview/issues/645)

</details>

<b><details><summary>69. 加减运算</summary></b>

参考答案：

``` js
alert("5" + 3); //53 string
alert("5" + "3"); //53 string
alert("5" - 3); //2 number
alert("5" - "3"); //2 number
```

[参与互动](https://github.com/yisainan/web-interview/issues/646)

</details>

<b><details><summary>70. 结果是什么？</summary></b>

参考答案：

``` js
function foo() {
    foo.a = function() {
        alert(1);
    };
    this.a = function() {
        alert(2);
    };
    a = function() {
        alert(3);
    };
    var a = function() {
        alert(4);
    };
}
foo.prototype.a = function() {
    alert(5);
};
foo.a = function() {
    alert(6);
};
foo.a(); //6
var obj = new foo();
obj.a(); //2
foo.a(); //1
```

[参与互动](https://github.com/yisainan/web-interview/issues/647)

</details>

<b><details><summary>71. 输出结果</summary></b>

参考答案：

``` js
var a = 5;

function test() {
    a = 0;
    alert(a);
    alert(this.a); //没有定义 a这个属性
    var a;
    alert(a);
}
test(); // 0, 5, 0
new test(); // 0, undefined, 0 //由于类它自身没有属性a， 所以是undefined
```

[参与互动](https://github.com/yisainan/web-interview/issues/648)

</details>

<b><details><summary>72. 计算字符串字节数</summary></b>

参考答案：

``` js
new(function(s) {
    if (!arguments.length || !s) return null;
    if ("" == s) return 0;
    var l = 0;
    for (var i = 0; i < s.length; i++) {
        if (s.charCodeAt(i) > 255) l += 2;
        else l += 1; //charCodeAt()得到的是unCode码
    } //汉字的unCode码大于 255bit 就是两个字节
    alert(l);
})("hello world!");
```

[参与互动](https://github.com/yisainan/web-interview/issues/649)

</details>

<b><details><summary>73. 输出结果</summary></b>

参考答案：

var bool = !!2;  alert(bool); //true; 
双向非操作可以把字符串和数字转换为布尔值

[参与互动](https://github.com/yisainan/web-interview/issues/650)

</details>

<b><details><summary>74. 声明对象，添加属性，输出属性</summary></b>

参考答案：

``` js
var obj = {
    name: "leipeng",
    showName: function() {
        alert(this.name);
    }
};
obj.showName();
```

[参与互动](https://github.com/yisainan/web-interview/issues/651)

</details>

<b><details><summary>75. 匹配输入的字符：第一个必须是字母或下划线开头，长度 5-20</summary></b>

参考答案：

``` js
var reg = /^[a-zA-Z][a-zA-Z0-9_]{5,20}/,
    name1 = "leipeng",
    name2 = "0leipeng",
    name3 = "你好leipeng",
    name4 = "hi";
alert(reg.test(name1));
alert(reg.test(name2));
alert(reg.test(name3));
alert(reg.test(name4));
```

[参与互动](https://github.com/yisainan/web-interview/issues/652)

</details>

<b><details><summary>76. 检测变量类型</summary></b>

参考答案：

``` js
function checkStr(str) {
    typeof str == "string" ? alert("true") : alert("false");
}
checkStr("leipeng");
```

[参与互动](https://github.com/yisainan/web-interview/issues/653)

</details>

<b><details><summary>77. 如何在 HTML 中添加事件，几种方法？</summary></b>

参考答案：

``` 
1、标签之中直接添加 onclick="fun()";
2、JS 添加 Eobj.onclick = method;
3、现代事件  IE： obj.attachEvent('onclick', method);
            FF: obj.addEventListener('click', method, false);
```

[参与互动](https://github.com/yisainan/web-interview/issues/654)

</details>

<b><details><summary>78. 请问代码实现 outerHTML</summary></b>

参考答案：

``` js
//说明：outerHTML其实就是innerHTML再加上本身；
Object.prototype.outerHTML = function() {
    var innerCon = this.innerHTML, //获得里面的内容
        outerCon = this.appendChild(innerCon); //添加到里面
    alert(outerCon);
};
```

演示代码：

```html     
<! DOCTYPE html>
<html>
  <head>

    <meta charset="UTF-8" />
    <title>Document</title>

  </head>
  <body>

    <div id="outer">
      hello
    </div>
    <script>
      Object.prototype.outerHTML = function() {
        var innerCon = this.innerHTML, //获得里面的内容
          outerCon = this.appendChild(innerCon); //添加到里面
        alert(outerCon);
      };
      function $(id) {
        return document.getElementById(id);
      }
      alert($("outer").innerHTML);
      alert($("outer").outerHTML);
    </script>

  </body>
</html>

``` 

[参与互动](https://github.com/yisainan/web-interview/issues/655)

</details>

<b><details><summary>79.JS 中的简单继承 call 方法</summary></b>

参考答案：

```js
//顶一个父母类，注意：类名都是首字母大写的哦！
function Parent(name, money) {
  this.name = name;
  this.money = money;
  this.info = function() {
    alert("姓名： " + this.name + " 钱： " + this.money);
  };
} //定义孩子类
function Children(name) {
  Parent.call(this, name); //继承 姓名属性，不要钱。
  this.info = function() {
    alert("姓名： " + this.name);
  };
} //实例化类
var per = new Parent("parent", 800000000000);
var chi = new Children("child");
per.info();
chi.info();
```

[参与互动](https://github.com/yisainan/web-interview/issues/656)

</details>

<b><details><summary>80. 解析 URL 成一个对象？</summary></b>

参考答案：

``` js
String.prototype.urlQueryString = function() {
    var url = this.split("?")[1].split("&"),
        len = url.length;
    this.url = {};
    for (var i = 0; i < len; i += 1) {
        var cell = url[i].split("="),
            key = cell[0],
            val = cell[1];
        this.url["" + key + ""] = val;
    }
    return this.url;
};
var url = "?name=12&age=23";
console.log(url.urlQueryString().age);
```

[参与互动](https://github.com/yisainan/web-interview/issues/657)

</details>

<b><details><summary>81. 看下列代码输出什么？</summary></b>

参考答案：

``` js
var foo = "11" + 2 - "1";
console.log(foo);
console.log(typeof foo);
// 执行完后foo的值为111，foo的类型为Number。
```

[参与互动](https://github.com/yisainan/web-interview/issues/658)

</details>

<b><details><summary>82. 看下列代码, 输出什么？</summary></b>

参考答案：

``` js
var a = new Object();
a.value = 1;
b = a;
b.value = 2;
alert(a.value);
// 执行完后输出结果为2
```

[参与互动](https://github.com/yisainan/web-interview/issues/659)

</details>

<b><details><summary>83. 已知数组 var stringArray = ["This", "is", "Baidu", "Campus"]，Alert 出"This is Baidu Campus"。</summary></b>

参考答案：alert(stringArray. join(""))

[参与互动](https://github.com/yisainan/web-interview/issues/660)

</details>

<b><details><summary>84. 请描述出下列代码运行的结果</summary></b>

参考答案：

``` js
function d() {
    console.log(this);
}
d();
```

[参与互动](https://github.com/yisainan/web-interview/issues/661)

</details>

<b><details><summary>85. 需要将变量 e 的值修改为"a+b+c+d", 请写出对应的代码</summary></b>

var e="abcd"; 

参考答案：

e. split(''). join('+')

[参与互动](https://github.com/yisainan/web-interview/issues/662)

</details>

<b><details><summary>86. 设计一段代码能够遍历下列整个 DOM 节点</summary></b>

``` html
<div>
    <p>
        <span><a></a></span>
        <span><a></a></span>
    </p>
    <ul>
        <li></li>
        <li></li>
    </ul>
</div>
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/663)

</details>

<b><details><summary>87. 怎样实现两栏等高？</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/664)

</details>

<b><details><summary>88. 使用 js 实现这样的效果：在文本域里输入文字时，当按下 enter 键时不换行，而是替换成"{{enter}}", (只需要考虑在行尾按下 enter 键的情况). </summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/665)

</details>

<b><details><summary>89. 以下代码中 end 字符串什么时候输出</summary></b>

``` js
var t = true;
setTimeout(function() {
    console.log(123);
    t = false;
}, 1000);
while (t) {}
console.log("end");
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/666)

</details>

<b><details><summary>90. specify('hello, world')//=>'h, e, l, l, o, w, o, r, l, d'实现 specify 函数</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/667)

</details>

<b><details><summary>91. 请将一个 URL 的 search 部分参数与值转换成一个 json 对象</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/668)

</details>

<b><details><summary>92. 请用原生 js 实现 jquery 的 get\post 功能，以及跨域情况下</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/669)

</details>

<b><details><summary>93. 请写出三种以上的 Firefox 有但 IE 没有的属性和函数</summary></b>

参考答案：

1、在 IE 下可通过 `document.frames["id"];` 得到该 IFRAME 对象，

而在火狐下则是通过 `document.getElementById("content_panel_if").contentWindow;` 

2、IE 的写法： `_tbody=_table.childNodes[0]` 
在 FF 中，firefox 会在子节点中包含空白则第一个子节点为空白""， 而 ie 不会返回空白
可以通过 `if("" != node.nodeName)` 过滤掉空白子对象

3、模拟点击事件

``` js
if (document.all) {
    //ie下
    document.getElementById("a3").click();
} else {
    //非IE
    var evt = document.createEvent("MouseEvents");
    evt.initEvent("click", true, true);
    document.getElementById("a3").dispatchEvent(evt);
}
```

4、事件注册

``` js
if (isIE) {
    window.attachEvent("onload", init);
} else {
    window.addEventListener("load", init, false);
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/670)

</details>

<b><details><summary>94. 写出 3 个使用 this 的典型应用</summary></b>

参考答案：

（1）、在 html 元素事件属性中使用，如：

``` html
<input type="button" οnclick="showInfo(this);" value="点击一下" />
```

（2）、构造函数

``` js
function Animal(name, color) {
    this.name = name;
    this.color = color;
}
```

（3）、input 点击，获取值

``` js
< input type = "button"
id = "text"
value = "点击一下" / >
    <
    script type = "text/javascript" >
    var btn = document.getElementById("text");
btn.onclick = function() {
        alert(this.value); //此处的this是按钮元素
    } <
    /script>
```

(4)、apply()/call()求数组最值

``` js
var numbers = [5, 458, 120, -215];
var maxInNumbers = Math.max.apply(this, numbers);
console.log(maxInNumbers); // 458
var maxInNumbers = Math.max.call(this, 5, 458, 120, -215);
console.log(maxInNumbers); // 458
```

[参与互动](https://github.com/yisainan/web-interview/issues/671)

</details>

<b><details><summary>95. 下面这个 ul，如何点击每一列的时候 alert 其 index?（闭包）</summary></b>

``` html
<ul id="test">
    <li>这是第一条</li>
    <li>这是第二条</li>
    <li>这是第三条</li>
</ul>
```

参考答案：

``` js
// 方法一：
var lis = document.getElementById("test").getElementsByTagName("li");
for (var i = 0; i < 3; i++) {
    lis[i].index = i;
    lis[i].onclick = function() {
        alert(this.index);
    };
}
//方法二：
var lis = document.getElementById("test").getElementsByTagName("li");
for (var i = 0; i < 3; i++) {
    lis[i].index = i;
    lis[i].onclick = (function(a) {
        return function() {
            alert(a);
        };
    })(i);
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/672)

</details>

<b><details><summary>96. 小贤是一条可爱的小狗(Dog)，它的叫声很好听(wow)，每次看到主人的时候就会乖乖叫一声(yelp)。从这段描述可以得到以下对象：</summary></b>

参考答案：

``` js
function Dog() {
    this.wow = function() {
        alert("Wow");
    };
    this.yelp = function() {
        this.wow();
    };
}
```

小芒和小贤一样，原来也是一条可爱的小狗，可是突然有一天疯了(MadDog)，一看到人就会每隔半秒叫一声(wow)地不停叫唤(yelp)。请根据描述，按示例的形式用代码来实。（继承，原型，setInterval）

``` js
function MadDog() {
    this.yelp = function() {
        var self = this;
        setInterval(function() {
            self.wow();
        }, 500);
    };
}
MadDog.prototype = new Dog();
//for test
var dog = new Dog();
dog.yelp();
var madDog = new MadDog();
madDog.yelp();
```

[参与互动](https://github.com/yisainan/web-interview/issues/673)

</details>

<b><details><summary>97. 实现一个函数 clone，可以对 JavaScript 中的 5 种主要的数据类型（包括 Numer、String、Object、Array、Boolean）进行值复制</summary></b>

参考答案：

* 察点 1：对于基本数据类型和引用数据类型在内存中存放的是值还是指针这一区别是否清楚
* 察点 2：是否知道如何判断一个变量是什么类型的
* 考察点 3：递归算法的设计

``` js
// 方法一：
Object.prototype.clone = function() {
    var o = this.constructor === Array ? [] : {};
    for (var e in this) {
        o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
    }
    return o;
};
/**
 * 克隆一个对象
 * @param Obj
 * @returns
 */
//方法二：
function clone(Obj) {
    var buf;
    if (Obj instanceof Array) {
        buf = []; //创建一个空的数组
        var i = Obj.length;
        while (i--) {
            buf[i] = clone(Obj[i]);
        }
        return buf;
    } else if (Obj instanceof Object) {
        buf = {}; //创建一个空对象
        for (var k in Obj) {
            //为这个对象添加新的属性
            buf[k] = clone(Obj[k]);
        }
        return buf;
    } else {
        //普通变量直接赋值
        return Obj;
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/674)

</details>

<b><details><summary>98. 用 js 实现随机选取 10–100 之间的 10 个数字，存入一个数组，并排序。</summary></b>

参考答案：

``` js
var iArray = [];
funtion getRandom(istart, iend) {
        var iChoice = istart - iend + 1;
        return Math.floor(Math.random() * iChoice + istart;
        }
        for (var i = 0; i < 10; i++) {
            iArray.push(getRandom(10, 100));
        }
        iArray.sort();
```

[参与互动](https://github.com/yisainan/web-interview/issues/675)

</details>

<b><details><summary>99. 输出今天的日期，以 YYYY-MM-DD 的方式，比如今天是 2014 年 9 月 26 日，则输出 2014-09-26</summary></b>

参考答案：

``` js
var d = new Date();
// 获取年，getFullYear()返回4位的数字
var year = d.getFullYear();
// 获取月，月份比较特殊，0是1月，11是12月
var month = d.getMonth() + 1;
// 变成两位
month = month < 10 ? "0" + month : month;
// 获取日
var day = d.getDate();
day = day < 10 ? "0" + day : day;
alert(year + "-" + month + "-" + day);
```

[参与互动](https://github.com/yisainan/web-interview/issues/676)

</details>

<b><details><summary>100. 写出函数 DateDemo 的返回结果，系统时间假定为今天</summary></b>

``` js
function DateDemo() {
    var d,
        s = "今天日期是：";
    d = new Date();
    s += d.getMonth() + "/";
    s += d.getDate() + "/";
    s += d.getYear();
    return s;
}
```

参考答案：今天日期是：7/17/2019

[参与互动](https://github.com/yisainan/web-interview/issues/677)

</details>

<b><details><summary>101. 下列 JavaScript 代码执行后，依次 alert 的结果是</summary></b>

``` js
var obj = {
    proto: {
        a: 1,
        b: 2
    }
};

function F() {}
F.prototype = obj.proto;
var f = new F();
obj.proto.c = 3;
obj.proto = {
    a: -1,
    b: -2
};
alert(f.a);
alert(f.c);
delete F.prototype["a"];
alert(f.a);
alert(obj.proto.a);
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/678)

</details>

<b><details><summary>102. 下列 JavaScript 代码执行后，运行的结果是</summary></b>

``` html
<button id="btn">点击我</button>
```

``` js
var btn = document.getElementById("btn");
var handler = {
    id: "_eventHandler",
    exec: function() {
        alert(this.id);
    }
};
btn.addEventListener("click", handler.exec.false);
```

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/679)

</details>

<b><details><summary>103. 输出结果是多少？</summary></b>

1）

``` js
var a; // undefined
var b = a * 0; // NaN
if (b == b) {
    // false
    console.log(b * 2 + "2" - 0 + 4);
} else {
    console.log(!b * 2 + "2" - 0 + 4); // 22 + 4 = 26
}
```

参考答案：26

2）

``` js
< script >
    var a = 1; <
/script> <
script >
    var a;
var b = a * 0;
if (b == b) { // true
    console.log(b * 2 + "2" - 0 + 4); // 6
} else {
    console.log(!b * 2 + "2" - 0 + 4);
} <
/script>
```

参考答案：6

3）

``` js
var t = 10;

function test(t) {
    var t = t++;
}
test(t);
console.log(t); // 外部不能访问函数内的变量
```

参考答案：10

4）

``` js
var t = 10;

function test(test) {
    var t = test++;
}
test(t);
console.log(t);
```

参考答案：10

6）

``` js
var t = 10;

function test(test) {
    t = test++;
    console.log(t);
}
test(t);
console.log(t);
```

参考答案：10

7）

``` js
var t = 10;

function test(test) {
    t = t + test;
    console.log(t);
    var t = 3;
}
test(t);
console.log(t);
```

参考答案：NaN 10

8）

``` js
var a;
var b = a / 0;
if (b == b) {
    console.log(b * 2 + "2" - 0 + 4);
} else {
    console.log(!b * 2 + "2" - 0 + 4);
}
```

参考答案：26

9）

``` js
< script >
    var a = 1; <
/script> <
script >
    var a;
var b = a / 0;
if (b == b) {
    console.log(b * 2 + "2" + 4);
} else {
    console.log(!b * 2 + "2" + 4);
} <
/script>
```

参考答案：Infinity24

[参与互动](https://github.com/yisainan/web-interview/issues/680)

</details>

<b><details><summary>104. 下列 JavaScript 代码执行后，iNum 的值是</summary></b>

``` js
var iNum = 0;
for (var i = 1; i < 10; i++) {
    if (i % 5 == 0) {
        continue;
    }
    iNum++;
}
console.log(iNum);
```

参考答案：8

[参与互动](https://github.com/yisainan/web-interview/issues/681)

</details>

<b><details><summary>105. 下列 JavaScript 代码执行后，依次打印的结果是</summary></b>

``` js
(function test() {
    var a = (b = 5);
    console.log(typeof a);
    console.log(typeof b);
})();
console.log(typeof a);
console.log(typeof b);
```

参考答案：

``` 
number
number
undefined
number
```

[参与互动](https://github.com/yisainan/web-interview/issues/682)

</details>

<b><details><summary>106. 不用任何插件，如何实现一个 tab 栏切换？</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/683)

</details>

<b><details><summary>107. js 中如何实现一个 map</summary></b>

参考答案：

``` 
数组的map方法：

概述
map() 方法返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。

语法
array.map(callback[, thisArg])

参数
callback

原数组中的元素经过该方法后返回一个新的元素。

currentValue

callback 的第一个参数，数组中当前被传递的元素。

index

callback 的第二个参数，数组中当前被传递的元素的索引。

array

callback 的第三个参数，调用 map 方法的数组。

thisArg

执行 callback 函数时 this 指向的对象。
```

实现：

``` js
Array.prototype.map2 = function(callback) {
    for (var i = 0; i < this.length; i++) {
        this[i] = callback(this[i]);
    }
};
```

[参与互动](https://github.com/yisainan/web-interview/issues/684)

</details>

<b><details><summary>108. 有 1 到 10w 这个 10w 个数，去除 2 个并打乱次序，如何找出那两个数？</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/685)

</details>

<b><details><summary>109. 如何获取对象 a 拥有的所有属性（可枚举的、不可枚举的，不包括继承来的属性）</summary></b>

参考答案：

``` js
Object.keys—— IE9 +
```

或者使用 for…in 并过滤出继承的属性

``` js
for (o in obj) {
    if (obj.hasOwnproperty(o)) {
        //把o这个属性放入到一个数组中
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/686)

</details>

<b><details><summary>110. 约瑟夫环—已知 n 个人（以编号 1，2，3…分别表示）围坐在一张圆桌周围。从编号为 k 的人开始报数，数到 m 的那个人出列；他的下一个人又从 1 开始报数，数到 m 的那个人又出列；依此规律重复下去，直到圆桌周围的人全部出列。</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/687)

</details>

<b><details><summary>111. FF 与 IE 中如何阻止事件冒泡，如何获取事件对象，以及如何获取触发事件的元素</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/688)

</details>

<b><details><summary>112. 下列控制台都输出什么</summary></b>

参考答案：

第 1 题：

``` js
function setName() {
    name = "张三";
}
setName();
console.log(name);
```

参考答案："张三"

第 2 题：

``` js
//考点：1、变量声明提升 2、变量搜索机制
var a = 1;

function test() {
    console.log(a);
    var a = 1;
}
test();
```

参考答案：undefined

第 3 题：

``` js
var b = 2;

function test2() {
    window.b = 3;
    console.log(b);
}
test2();
```

参考答案：3

第 4 题：

``` js
c = 5; //声明一个全局变量c
function test3() {
    window.c = 3;
    console.log(c); //参考答案：undefined，原因：由于此时的c是一个局部变量c，并且没有被赋值
    var c;
    console.log(window.c); //参考答案：3，原因：这里的c就是一个全局变量c
}
test3();
```

第 5 题：

``` js
var arr = [];
arr[0] = "a";
arr[1] = "b";
arr[10] = "c";
alert(arr.length); //参考答案：11
console.log(arr[5]); //参考答案：undefined
```

第 6 题：

``` js
var a = 1;
console.log(a++); //参考答案：1
console.log(++a); //参考答案：3
```

第 7 题：

``` js
console.log(null == undefined); //参考答案：true
console.log("1" == 1); //参考答案：true，因为会将数字1先转换为字符串1
console.log("1" === 1); //参考答案：false，因为数据类型不一致
```

第 8 题：

``` js
typeof 1;
("number");
typeof "hello";
("string");
typeof /[0-9]/;
("object");
typeof {};
("object");
typeof null;
("object");
typeof undefined;
("undefined");
typeof [1, 2, 3];
("object");
typeof

function() {}; //"function"
```

第 9 题：

``` js
parseInt(3.14); //3
parseFloat("3asdf"); //3
parseInt("1.23abc456");
parseInt(true); //"true" NaN
```

第 10 题：

``` js
//考点：函数声明提前
function bar() {
    return foo;
    foo = 10;

    function foo() {}
    //var foo = 11;
}
alert(typeof bar()); //"function"
```

第 11 题：考点：函数声明提前

``` js
var foo = 1;

function bar() {
    foo = 10;
    return;

    function foo() {}
}
bar();
alert(foo); //参考答案：1
```

第 12 题：

``` js
console.log(a); //是一个函数
var a = 3;

function a() {}
console.log(a); ////3
```

第 13 题：

``` js
//考点：对arguments的操作
function foo(a) {
    arguments[0] = 2;
    alert(a); //参考答案：2，因为：a、arguments是对实参的访问，b、通过arguments[i]可以修改指定实参的值
}
foo(1);
265、 第14题：
function foo(a) {
    alert(arguments.length); //参考答案：3，因为arguments是对实参的访问
}
foo(1, 2, 3);
```

第 15 题

``` js
bar(); //报错
var foo = function bar(name) {
    console.log("hello" + name);
    console.log(bar);
};
//alert(typeof bar);
foo("world"); //"hello"
console.log(bar); //undefined
console.log(foo.toString());
bar(); //报错
```

第 16 题

``` js
function test() {
    console.log("test函数");
}
setTimeout(function() {
    console.log("定时器回调函数");
}, 0);
test();

function foo() {
    var name = "hello";
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/689)

</details>

<b><details><summary>113. console. log( 8 | 1 ); 输出值是多少？</summary></b>

参考答案：9

[参与互动](https://github.com/yisainan/web-interview/issues/690)

</details>

<b><details><summary>114. 只允许使用 + - _ / 和 Math. _ ，求一个函数 y = f(x, a, b); 当 x > 100 时返回 a 的值，否则返回 b 的值，不能使用 if else 等条件语句，也不能使用|, ?:, 数组。</summary></b>

参考答案：

``` js
function f(x, a, b) {
    var temp = Math.ceil(Math.min(Math.max(x - 100, 0), 1));
    return a * temp + b * (1 - temp);
}
console.log(f(-10, 1, 2));
```

[参与互动](https://github.com/yisainan/web-interview/issues/691)

</details>

<b><details><summary>115. JavaScript alert(0. 4\*0. 2); 结果是多少？和你预期的一样吗？如果不一样该如何处理？</summary></b>

参考答案：有误差，应该比准确结果偏大。 一般我会将小数变为整数来处理。当前之前遇到这个问题时也上网查询发现有人用 try catch return 写了一个函数，
当然原理也是一致先转为整数再计算。看起来挺麻烦的，我没用过。

[参与互动](https://github.com/yisainan/web-interview/issues/692)

</details>

<b><details><summary>116. 如何显示/隐藏一个 dom 元素？请用原生的 JavaScript 方法实现</summary></b>

参考答案：

dom. style. display="none"; 
dom. style. display="block"; 

[参与互动](https://github.com/yisainan/web-interview/issues/693)

</details>

<b><details><summary>117. 编写一个 JavaScript 函数，输入指定类型的选择器(仅需支持 id，class，tagName 三种简单 CSS 选择器，无需兼容组合选择器)可以返回匹配的 DOM 节点，需考虑浏览器兼容性和性能。</summary></b>

参考答案：

``` js
/*** @param selector {String} 传入的CSS选择器。* @return {Array}*/
var query = function(selector) {
    var reg = /^(#)?(\.)?(\w+)$/gim;
    var regResult = reg.exec(selector);
    var result = [];
    //如果是id选择器
    if (regResult[1]) {
        if (regResult[3]) {
            if (typeof document.querySelector === "function") {
                result.push(document.querySelector(regResult[3]));
            } else {
                result.push(document.getElementById(regResult[3]));
            }
        }
    } //如果是class选择器
    else if (regResult[2]) {
        if (regResult[3]) {
            if (typeof document.getElementsByClassName === "function") {
                var doms = document.getElementsByClassName(regResult[3]);
                if (doms) {
                    result = converToArray(doms);
                }
            } //如果不支持getElementsByClassName函数
            else {
                var allDoms = document.getElementsByTagName("*");
                for (var i = 0, len = allDoms.length; i < len; i++) {
                    if (allDoms[i].className.search(new RegExp(regResult[2])) > -1) {
                        result.push(allDoms[i]);
                    }
                }
            }
        }
    } //如果是标签选择器
    else if (regResult[3]) {
        var doms = document.getElementsByTagName(regResult[3].toLowerCase());
        if (doms) {
            result = converToArray(doms);
        }
    }
    return result;
};

function converToArray(nodes) {
    var array = null;
    try {
        array = Array.prototype.slice.call(nodes, 0); //针对非IE浏览器
    } catch (ex) {
        array = new Array();
        for (var i = 0, len = nodes.length; i < len; i++) {
            array.push(nodes[i]);
        }
    }
    return array;
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/694)

</details>

<b><details><summary>118. 如现在有一个效果，有显示用户头像、用户昵称、用户其他信息；当用户鼠标移到头像上时，会弹出用户的所有信息；如果是你，你会如何实现这个功能，请用代码实现？</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/695)

</details>

<b><details><summary>119. call 与 apply 有什么作用？又有什么什么区别？用 callee 属性实现函数递归？</summary></b>

参考答案：apply 的参数是数组, call 的参数是单个的值，除此之外，两者没有差别，重点理解 this 的改变，callee 已经不推荐使用

[参与互动](https://github.com/yisainan/web-interview/issues/696)

</details>

<b><details><summary>120. 用正则表达式，写出由字母开头，其余由数字、字母、下划线组成的 6~30 的字符串？</summary></b>

参考答案：var reg=/^[a-ZA-Z][\da-za-z_]{5, 29}/; 

[参与互动](https://github.com/yisainan/web-interview/issues/697)

</details>

<b><details><summary>121. 写一个函数可以计算 sum(5, 0, -5); 输出 0; sum(1, 2, 3, 4); 输出 10; </summary></b>

参考答案：

``` js
function calc() {
    var result = 0;
    for (var i = 0; i < arguments.length; i++) {
        var obj = arguments[i];
        result += obj;
    }
    return result;
}
alert(calc(1, 2, 3, 4));
```

[参与互动](https://github.com/yisainan/web-interview/issues/698)

</details>

<b><details><summary>122. 请评价以下代码并给出改进意见</summary></b>

参考答案：

``` js
if (window.addEventListener) {
    var addListener = function(el, type, listener, useCapture) {
        el.addEventListener(type, listener, useCapture);
    };
} else if (document.all) {
    addListener = function(el, type, listener) {
        el.attachEvent("on" + type, function() {
            listener.apply(el);
        });
    };
}
```

*　不应该在 if 和 else 语句中声明 addListener 函数，应该先声明；
*　不需要使用 window. addEventListener 或 document. all 来进行检测浏览器，应该使用能力检测； \*　由于 attachEvent 在 IE 中有 this 指向问题，所以调用它时需要处理一下

改进如下：

``` js
function addEvent(elem, type, handler) {
    if (elem.addEventListener) {
        elem.addEventListener(type, handler, false);
    } else if (elem.attachEvent) {
        elem["temp" + type + handler] = handler;
        elem[type + handler] = function() {
            elem["temp" + type + handler].apply(elem);
        };
        elem.attachEvent("on" + type, elem[type + handler]);
    } else {
        elem["on" + type] = handler;
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/699)

</details>

<b><details><summary>123. 对于 apply 和 call 两者在作用上是相同的，即是调用一个对象的一个方法，以另一个对象替换当前对象。将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。</summary></b>

参考答案：但两者在参数上有区别的。对于第一个参数意义都一样，但对第二个参数：?apply 传入的是一个参数数组，也就是将多个参数组合成为一个数组传入，而 call 则作为 call 的参数传入（从第二个参数开始）。? 如 func. call(func1, var1, var2, var3)对应的 apply 写法为：func. apply(func1, [var1, var2, var3]) 。

[参与互动](https://github.com/yisainan/web-interview/issues/700)

</details>

<b><details><summary>124. 《正则》写出正确的正则表达式匹配固话号，区号 3-4 位，第一位为 0，中横线，7-8 位数字，中横线，3-4 位分机号格式的固话号</summary></b>

参考答案：/0[0-9]{2, 3}-\d{7, 8}/

[参与互动](https://github.com/yisainan/web-interview/issues/701)

</details>

<b><details><summary>125. 《算法》 一下 A, B 可任选一题作答，两题全答加分</summary></b>

A: 农场买了一只羊，第一年是小羊，第二年底生一只，第三年不生，第四年底再生一只，第五年死掉。

B: 写出代码对下列数组去重并从大到小排列{5, 2, 3, 6, 8, 6, 5, 4, 7, 1, 9}

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/702)

</details>

<b><details><summary>126. 给 String 对象添加一个方法，传入一个 string 类型的参数，然后将 string 的每个字符间加个空格返回，例如：addSpace("hello world") // -> 'h e l l o  w o r l d'</summary></b>

参考答案：

``` js
String.prototype.spacify = function() {
    return this.split("").join(" ");
};
```

接着上述问题参考答案提问，1）直接在对象的原型上添加方法是否安全？尤其是在 Object 对象上。(这个我没能答出？希望知道的说一下。)　 2）函数声明与函数表达式的区别？

参考答案：在 js 中，解析器在向执行环境中加载数据时，对函数声明和函数表达式并非是一视同仁的，解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问），至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解析执行。

[参与互动](https://github.com/yisainan/web-interview/issues/703)

</details>

<b><details><summary>127. 请写一个正则表达式：要求最短 6 位数，最长 20 位，阿拉伯数和英文字母（不区分大小写）组成</summary></b>

参考答案：^(?=. *\d)(?=. *[a-z])(?=. *[A-Z])[a-zA-Z\d]{6, 20}$

[参与互动](https://github.com/yisainan/web-interview/issues/704)

</details>

<b><details><summary>128. 统计 1 到 400 亿之间的自然数中含有多少个 1？比如 1-21 中，有 1、10、11、21 这四个自然数有 13 个 1</summary></b>

参考答案：

归纳法

归纳法，对于个位出现的1：(n / 10) * 1 + (n % 10) >= 1 ? 1 : 0;
对于十位出现的1：(n / 100) * 10 + if (k > 19) 10 else if (k < 10) 0 else k - 10 + 1;
对于百位出现的1：(n / 1000) * 100 + if (k > 199) 10 else if (k < 100) 0 else k - 100 + 1;
最终归纳出: (n / (i * 10)) * i + if (k > 2 * i - 1) i else if (k < i) 0 else k - i + 1, 其中k = n % (i * 10);

代码：

```js
var countDigitOne = function(n) {
    let count = 0;

    for (let i = 1; i <= n; i *= 10) {
        let divide = i * 10;
        let p = Math.floor(n / divide), k = n % divide, rest = 0;

        count += p * i;
        rest = (k > (2 * i - 1)) ? i : ((k < i) ? 0 : k - i + 1);
        count += rest;
    }
    return count;
};
countDigitOne(40000000000)
```

[参与互动](https://github.com/yisainan/web-interview/issues/705)

</details>

<b><details><summary>129. 删除与某个字符相邻且相同的字符，比如 fdaffdaaklfjklja 字符串处理之后成为"fdafdaklfjklja"</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/706)

</details>

<b><details><summary>130. 请写出一个程序，在页面加载完成后动态创建一个 form 表单，并在里面添加一个 input 对象并给它任意赋值后义 post 方式提交到：http://127.0.0.1/save.php</summary></b>

参考答案：

[参与互动](https://github.com/yisainan/web-interview/issues/707)

</details>

<b><details><summary>131. 定义一个 log 方法，让它可以代理 console. log 的方法。</summary></b>

参考答案：

可行的方法一：

``` js
function log(msg) {
    console.log(msg);
}
log("hello world!"); // hello world!
```

如果要传入多个参数呢？显然上面的方法不能满足要求，所以更好的方法是：

``` js
function log() {
    console.log.apply(console, arguments);
}
```

到此，追问 apply 和 call 方法的异同。

对于 apply 和 call 两者在作用上是相同的，即是调用一个对象的一个方法，以另一个对象替换当前对象。将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。

但两者在参数上有区别的。对于第一个参数意义都一样，但对第二个参数： apply 传入的是一个参数数组，也就是将多个参数组合成为一个数组传入，而 call 则作为 call 的参数传入（从第二个参数开始）。  如 func. call(func1, var1, var2, var3)对应的 apply 写法为：func. apply(func1, [var1, var2, var3]) 。

[参与互动](https://github.com/yisainan/web-interview/issues/708)

</details>

<b><details><summary>132. 编写一个快速方法将 html 的 sup 提取转换为一个数组</summary></b>

``` js
// 编写一个快速方法将html的sup提取转换为一个数组，如：
let str = "气量(10<sup>8</sup>m<sup>3</sup>)";
// 输出结果
// ['气量(10',8,'m',3,')']
```

参考答案：

``` js
// 方法1
str.split(/\<\/?sup\>/);
// 方法2
str.split(/<[^>]+>/);
```

[参与互动](https://github.com/yisainan/web-interview/issues/709)

</details>

<b><details><summary>133. 求 num 的值</summary></b>

参考答案：

``` js
// 面试题1
var num = 123;

function f1() {
    console.log(num); // 123
}

function f2() {
    var num = 456;
    f1();
}
f2();

// 面试题1 变式
var num = 123;

function f1(num) {
    console.log(num); // 456
}

function f2() {
    var num = 456;
    f1(num);
}
f2();

// 面试题1 变式
var num = 123;

function f1() {
    console.log(num); // 456
}
f2();

function f2() {
    num = 456; //这里是全局变量
    f1();
}
console.log(num); // 456
```

[参与互动](https://github.com/yisainan/web-interview/issues/710)

</details>

<b><details><summary>134. 有一个函数，参数是一个函数，返回值也是一个函数，返回的函数功能和入参的函数相似，但这个函数只能执行 3 次，再次执行无效，如何实现</summary></b>

这个题目是考察闭包的使用

参考答案：

``` js
function sayHi() {
    console.log("hi");
}

function threeTimes(fn) {
    let times = 0;
    return () => {
        if (times++ < 3) {
            fn();
        }
    };
}

const newFn = threeTimes(sayHi);
newFn();
newFn();
newFn();
newFn();
newFn(); // 后面两次执行都无任何反应
```

通过闭包变量 `times` 来控制函数的执行

[参与互动](https://github.com/yisainan/web-interview/issues/711)

</details>

<b><details><summary>135. 实现 add 函数, 让 add(a)(b)和 add(a, b)两种调用结果相同</summary></b>

参考答案：

``` js
function add(a, b) {
    if (b === undefined) {
        return function(x) {
            return a + x;
        };
    }
    return a + b;
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/712)

</details>

<b><details><summary>136. 格式化金钱，每千分位加逗号</summary></b>

参考答案：

``` js
function format(str) {
    let s = "";
    let count = 0;
    for (let i = str.length - 1; i >= 0; i--) {
        s = str[i] + s;
        count++;
        if (count % 3 == 0 && i != 0) {
            s = "," + s;
        }
    }
    return s;
}
```

``` js
function format(str) {
    return str.replace(/(\d)(?=(?:\d{3})+$)/g, "$1,");
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/713)

</details>

<b><details><summary>137. 反转数组</summary></b>

### 要求

**input**: I am a student <br>
**output**: student a am I <br>
输入是数组 输出也是数组<br>
不允许用 `split`  `splice`  `reverse` <br>

参考答案：

#### 解法一

``` js
function reverseArry(arry) {
    const str = arry.join(" ");
    const result = [];
    let word = "";
    for (let i = 0, len = str.length; i < len; i++) {
        if (str[i] != " ") {
            word += str[i];
        } else {
            result.unshift(word);
            word = "";
        }
    }

    result.unshift(word);
    return result;
}

console.log(reverseArry(["I", "am", "a", "student"]));
// ["student", "a", "am", "I"]
```

#### 解法二

``` js
function reverseArry(arry) {
    const result = [];
    const distance = arry.length - 1;
    for (let i = distance; i >= 0; i--) {
        result[distance - i] = arry[i];
    }

    return result;
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/714)

</details>

<b><details><summary>138. 说出以下函数的作用是？空白区域应该填写什么？</summary></b>

参考答案：

``` js
//define
(function(window) {
    function fn(str) {
        this.str = str;
    }
    fn.prototype.format = function() {
        var arg = ______;
        return this.str.replace(_____, function(a, b) {
            return arg[b] || "";
        });
    };
    window.fn = fn;
})(window);
//use
(function() {
    var t = new fn('<p><a href="{0}">{1}</a><span>{2}</span></p>');
    console.log(t.format("http://www.alibaba.com", "Alibaba", "Welcome"));
})();
```

参考答案：访函数的作用是使用 format 函数将函数的参数替换掉{0}这样的内容，返回一个格式化后的结果：
第一个空是：arguments
第二个空是：/\{(\d+)\}/ig

[参与互动](https://github.com/yisainan/web-interview/issues/715)

</details>

<b><details><summary>139. （设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生 JS）</summary></b>

参考答案：

回答出概念即可，下面是几个要点

1. 给需要拖拽的节点绑定 mousedown, mousemove, mouseup 事件
2. mousedown 事件触发后，开始拖拽
3. mousemove 时，需要通过 event.clientX 和 clientY 获取拖拽位置，并实时更新位置
4. mouseup 时，拖拽结束
5. 需要注意浏览器边界的情况

[参与互动](https://github.com/yisainan/web-interview/issues/716)

</details>

<b><details><summary>140. 原生 JS 的 window. onload 与 Jquery 的\$(document). ready(function(){})有什么不同？如何用原生 JS 实现 Jq 的 ready 方法？</summary></b>

参考答案：

window. onload()方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。
\$(document). ready()是 DOM 结构绘制完毕后就执行，不必等到加载完毕。

``` js
/*
 * 传递函数给whenReady()
 * 当文档解析完毕且为操作准备就绪时，函数作为document的方法调用
 */
var whenReady = (function() {
    //这个函数返回whenReady()函数
    var funcs = []; //当获得事件时，要运行的函数
    var ready = false; //当触发事件处理程序时,切换为true //当文档就绪时,调用事件处理程序
    function handler(e) {
        if (ready) return; //确保事件处理程序只完整运行一次 //如果发生onreadystatechange事件，但其状态不是complete的话,那么文档尚未准备好
        if (e.type === "onreadystatechange" && document.readyState !== "complete") {
            return;
        } //运行所有注册函数 //注意每次都要计算funcs.length //以防这些函数的调用可能会导致注册更多的函数
        for (var i = 0; i < funcs.length; i++) {
            funcs[i].call(document);
        } //事件处理函数完整执行,切换ready状态, 并移除所有函数
        ready = true;
        funcs = null;
    } //为接收到的任何事件注册处理程序
    if (document.addEventListener) {
        document.addEventListener("DOMContentLoaded", handler, false);
        document.addEventListener("readystatechange", handler, false); //IE9+
        window.addEventListener("load", handler, false);
    } else if (document.attachEvent) {
        document.attachEvent("onreadystatechange", handler);
        window.attachEvent("onload", handler);
    } //返回whenReady()函数
    return function whenReady(fn) {
        if (ready) {
            fn.call(document);
        } else {
            funcs.push(fn);
        }
    };
})();
```

如果上述代码十分难懂，下面这个简化版：

``` js
function ready(fn) {
    if (document.addEventListener) {
        //标准浏览器
        document.addEventListener(
            "DOMContentLoaded",
            function() {
                //注销事件, 避免反复触发
                document.removeEventListener(
                    "DOMContentLoaded",
                    arguments.callee,
                    false
                );
                fn(); //执行函数
            },
            false
        );
    } else if (document.attachEvent) {
        //IE
        document.attachEvent("onreadystatechange", function() {
            if (document.readyState == "complete") {
                document.detachEvent("onreadystatechange", arguments.callee);
                fn(); //函数执行
            }
        });
    }
}
```

[参与互动](https://github.com/yisainan/web-interview/issues/717)

</details>

<b><details><summary>141. 对作用域上下文和 this 的理解，看下列代码：</summary></b>

``` js
var User = {
    count: 1,
    getCount: function() {
        return this.count;
    }
};
console.log(User.getCount()); // what?
var func = User.getCount;
console.log(func()); // what?
```

问两处 console 输出什么？为什么？

参考答案：1 和 undefined。func 是在 winodw 的上下文中被执行的，所以会访问不到 count 属性。

解析：

继续追问，那么如何确保 Uesr 总是能访问到 func 的上下文，即正确返回 1。正确的方法是使用 Function. prototype. bind。兼容各个浏览器完整代码如下：

``` js
Function.prototype.bind =
    Function.prototype.bind ||
    function(context) {
        var self = this;
        return function() {
            return self.apply(context, arguments);
        };
    };
var func = User.getCount.bind(User);
console.log(func());
```

[参与互动](https://github.com/yisainan/web-interview/issues/718)

</details>

<b><details><summary>142. 手动封装一个请求函数，可以设置最大请求次数，请求成功则不再请求，请求失败则继续请求直到超过最大次数(流利说)</summary></b>

参考答案：

</details>

<b><details><summary>143. 实现一个函数判断数据类型(哔哩哔哩)</summary></b>

参考答案：

</details>

<b><details><summary>144. 对象数组如何去重？（烈熊网络）</summary></b>

参考答案：

</details>

<b><details><summary>145. 实现数组的filter方法</summary></b>

参考答案：

</details>

<b><details><summary>146. 实现数组的flat方法</summary></b>

参考答案：

</details>

<b><details><summary>147. 如何实现一个模板引擎</summary></b>

参考答案：

</details>

<b><details><summary>148. 实现一个最基本的数据绑定</summary></b>

参考答案：

</details>

<b><details><summary>149. 请实现一个函数，找出这个树形结构中所有child的name，返回数组</summary></b>

参考答案：

</details>

<b><details><summary>150. 解析 URL Params 为对象</summary></b>

参考答案：

</details>

<b><details><summary>151. 统计 1 ~ n 整数中出现 1 的次数。</summary></b>

参考答案：

</details>

<b><details><summary>152. 如何将 [{id: 1}, {id: 2, pId: 1}, ... ] 的重复数组（有重复数据）转成树形结构的数组 [{id: 1, child: [{id: 2, pId: 1}]}, ... ] （需要去重）</summary></b>

参考答案：

</details>

<b><details><summary>153. 扑克牌问题</summary></b>

参考答案：

</details>

<b><details><summary>154. 如何用 css 或 js 实现多行文本溢出省略效果，考虑兼容性</summary></b>

参考答案：

</details>

<b><details><summary>155. 实现一个 Dialog 类，Dialog可以创建 dialog 对话框，对话框支持可拖拽</summary></b>

参考答案：

</details>

<b><details><summary>156. 用 setTimeout 实现 setInterval，阐述实现的效果与setInterval的差异</summary></b>

参考答案：

</details>

<b><details><summary>157. 求两个日期中间的有效日期</summary></b>

参考答案：

</details>

<b><details><summary>158. （算法题）求多个数组之间的交集</summary></b>

参考答案：

</details>

<b><details><summary>159. 手写二进制转base64（阿里）</summary></b>

参考答案：

</details>

<b><details><summary>160. （算法题）求多个数组之间的交集</summary></b>

参考答案：

</details>

<b><details><summary>161. Async/Await 如何通过同步的方式实现异步</summary></b>

参考答案：

</details>

<b><details><summary>162. 模拟实现一个 localStorage</summary></b>

参考答案：

</details>

<b><details><summary>163. 模拟 localStorage 时如何实现过期时间功能</summary></b>

参考答案：

</details>

<b><details><summary>164. 考虑到性能问题，如何快速从一个巨大的数组中随机获取部分元素</summary></b>

参考答案：

</details>

<b><details><summary>165. 写一个单向链数据结构的 js 实现并标注复杂度</summary></b>

参考答案：

</details>

<b><details><summary>166. 使用 JavaScript Proxy 实现简单的数据绑定</summary></b>

参考答案：

</details>

<b><details><summary>167. 算法题「移动零」，给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。</summary></b>

参考答案：

</details>

<b><details><summary>168. 给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。请找出这两个有序数组的中位数。要求算法的时间复杂度为 O(log(m n))。</summary></b>

参考答案：

</details>

<b><details><summary>169. 某公司 1 到 12 月份的销售额存在一个对象里面，如下：{1:222, 2:123, 5:888}，请把数据处理为如下结构：[222, 123, null, null, 888, null, null, null, null, null, null, null]。</summary></b>

参考答案：

</details>

<b><details><summary>170. 模拟实现一个 Promise. finally</summary></b>

参考答案：

</details>

<b><details><summary>171. ES6 代码转成 ES5 代码的实现思路是什么</summary></b>

参考答案：

</details>

<b><details><summary>172. 随机生成一个长度为 10 的整数类型的数组，例如 [2, 10, 3, 4, 5, 11, 10, 11, 20]，将其排列成一个新数组，要求新数组形式如下，例如 [[2, 3, 4, 5], [10, 11], [20]]。</summary></b>

参考答案：

</details>

<b><details><summary>173. 如何把一个字符串的大小写取反（大写变小写小写变大写），例如 AbC 变成 aBc 。</summary></b>

参考答案：

</details>

<b><details><summary>174. 实现一个字符串匹配算法，从长度为 n 的字符串 S 中，查找是否存在字符串 T，T 的长度是 m，若存在返回所在位置。</summary></b>

参考答案：

</details>

<b><details><summary>175. setTimeout、Promise、Async/Await 的区别</summary></b>

参考答案：

</details>

<b><details><summary>176. 请把俩个数组 [A1, A2, B1, B2, C1, C2, D1, D2] 和 [A, B, C, D]，合并为 [A1, A2, A, B1, B2, B, C1, C2, C, D1, D2, D]。</summary></b>

参考答案：

</details>

<b><details><summary>177. 实现一个 sleep 函数，比如 sleep(1000) 意味着等待1000毫秒，可从 Promise、Generator、Async/Await 等角度实现</summary></b>

参考答案：

</details>

<b><details><summary>178. 使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果</summary></b>

参考答案：

</details>

<b><details><summary>179. 怎样实现千位分隔符</summary></b>

参考答案：

</details>

<b><details><summary>180. 请写一个正则，去除掉html标签字符串里的所有属性，并保留src和href两种属性</summary></b>

参考答案：

</details>

<b><details><summary>181. (开放题）2万小球问题：在浏览器端，用js存储2万个小球的信息，包含小球的大小，位置，颜色等，如何做到对这2万条小球信息进行最优检索和存储</summary></b>

参考答案：

</details>

<b><details><summary>182. 如何遍历一个dom树</summary></b>

参考答案：

</details>

<b><details><summary>183. 实现Array数组的flat方法</summary></b>

参考答案：

</details>

<b><details><summary>184. 获取元素的最终background-color</summary></b>

参考答案：

</details>

<b><details><summary>185. 10w 条记录的数组，一次性渲染到页面上，如何处理可以不冻结UI？</summary></b>

参考答案：

</details>

<b><details><summary>186. 说下 [1, 2, 3]. map(parseInt) 结果</summary></b>

参考答案：[1, NaN, NaN]

</details>

<b><details><summary>187. 写一个发布订阅 EventEmitter方法</summary></b>

参考答案：

</details>

<b><details><summary>188. 使用setTimeout代替setInterval进行间歇调用</summary></b>

参考答案：

</details>

<b><details><summary>189. 实现一个私有变量</summary></b>

参考答案：

</details>

<b><details><summary>190. 实现一个instanceOf</summary></b>

参考答案：

</details>

<b><details><summary>191. 实现一个JS函数柯里化</summary></b>

参考答案：

</details>

<b><details><summary>192. 手写一个继承</summary></b>

参考答案：

</details>

<b><details><summary>193. 实现一个JSON. parse</summary></b>

参考答案：

</details>

<b><details><summary>194. 实现一个JSON. stringify</summary></b>

参考答案：

</details>

<b><details><summary>195. 实现一个new操作符</summary></b>

参考答案：

</details>

<b><details><summary>196. 实现一个new操作符</summary></b>

参考答案：

</details>

<b><details><summary>197. 基本数据类型和引用类型在存储上的差别</summary></b>

参考答案：

</details>

<b><details><summary>198. 如何实现一个 apply 函数？</summary></b>

参考答案：

</details>

<b><details><summary>199. 如何实现一个 call 函数？</summary></b>

参考答案：

</details>

<b><details><summary>200. 如何实现一个 bind 函数？</summary></b>

参考答案：

</details>

<b><details><summary>201. 题目如下：可使用任意前端框架，实现一个页面</summary></b>

``` 
页面上至少包含两个element
一个input输入框，一个output[用什么容器，标签都可以]
input要求输入一个数，表示图形面积
output要求输出一个六芒星[必须自己用css+html画出来，不能使用第三方]，六芒星面积是input的数值。
当input发生改变时，输出图形使用css3新特性进行过渡。
【请注意考虑边界条件和异常输入】
加分项：input可以处理汉字表示的数字如：十一 十二
```

参考答案：

</details>

<b><details><summary>202. 下面的代码输出什么？</summary></b>

``` js
var y = 1;
if (function f() {}) {
    y += typeof f;
}
console.log(y);
```

参考答案：1undefined

JavaScript中if语句求值其实使用eval函数，eval(function f(){}) 返回 function f(){} 也就是 true。

下面我们可以把代码改造下，变成其等效代码。

``` js
var k = 1;
if (1) {
    eval(function foo() {});
    k += typeof foo;
}
console.log(k);
```

上面的代码输出其实就是 1undefined。为什么那？我们查看下 eval() 说明文档即可获得参考答案

该方法只接受原始字符串作为参数，如果 string 参数不是原始字符串，那么该方法将不作任何改变地返回。

恰恰 function f(){} 语句的返回值是 undefined，所以一切都说通了。

注意上面代码和以下代码不同。

``` js
var k = 1;
if (1) {
    function foo() {};
    k += typeof foo;
}
console.log(k); // output 1function
```

</details>

<b><details><summary>203. 写一个mul函数，使用方法如下。</summary></b>

``` js
console.log(mul(2)(3)(4)); // output : 24 
console.log(mul(4)(3)(4)); // output : 48
```

参考答案：

``` js
function mul(x) {
    return function(y) { // anonymous function 
        return function(z) { // anonymous function 
            return x * y * z;
        };
    };
}
```

简单说明下： mul 返回一个匿名函数，运行这个匿名函数又返回一个匿名函数，最里面的匿名函数可以访问 x, y, z 进而算出乘积返回即可。

</details>

<b><details><summary>204. 下面代码输出什么？</summary></b>

``` js
var output = (function(x) {
    delete x;
    return x;
})(0);

console.log(output);
```

参考答案：输出是 0。 delete 操作符是将object的属性删去的操作。但是这里的 x 是并不是对象的属性， delete 操作符并不能作用。

</details>

<b><details><summary>205. 下面代码输出什么？</summary></b>

``` js
var x = {
    foo: 1
};
var output = (function() {
    delete x.foo;
    return x.foo;
})();

console.log(output);
```

参考答案：输出是 undefined。x虽然是全局变量，但是它是一个object。delete作用在x. foo上，成功的将x. foo删去。所以返回undefined

</details>

<b><details><summary>206. 下面代码输出什么？</summary></b>

``` js
var Employee = {
    company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

参考答案：输出是 xyz，这里的 emp1 通过 prototype 继承了 Employee的 company。emp1自己并没有company属性。所以delete操作符的作用是无效的。

</details>

<b><details><summary>207. 下面代码输出什么？</summary></b>

``` js
var trees = ["xyz", "xxxx", "test", "ryan", "apple"];
delete trees[3];

console.log(trees.length);
```

参考答案：输出是5。因为delete操作符并不是影响数组的长度。

</details>

<b><details><summary>208. 下面代码输出什么？</summary></b>

``` js
var bar = true;
console.log(bar + 0);
console.log(bar + "xyz");
console.log(bar + true);
console.log(bar + false);
```

参考答案：

``` 
1
truexyz
2
1
```

</details>

<b><details><summary>209. 下面代码输出什么？</summary></b>

``` js
var z = 1,
    y = z = typeof y;
console.log(y);
```

参考答案：输出是 undefined。js中赋值操作结合律是右至左的 ，即从最右边开始计算值赋值给左边的变量。

</details>

<b><details><summary>210. 下面代码输出什么？</summary></b>

``` js
var foo = function bar() {
    return 12;
};
typeof bar();
```

参考答案：输出是抛出异常，bar is not defined。

如果想让代码正常运行，需要这样修改代码：

``` js
var bar = function() {
    return 12;
};
typeof bar();
```

或者是

``` js
function bar() {
    return 12;
};
typeof bar();
```

明确说明这个下问题

``` js
var foo = function bar() {
    // foo is visible here 
    // bar is visible here
    console.log(typeof bar()); // Work here :)
};
// foo is visible here
// bar is undefined here
```

</details>

<b><details><summary>207. 下面代码输出什么？</summary></b>

``` js
var salary = "1000$";

(function() {
    console.log("Original salary was " + salary);

    var salary = "5000$";

    console.log("My New Salary " + salary);
})();
```

参考答案：

``` 
Original salary was undefined
My New Salary 5000$
```

这题同样考察的是变量提升。等价于以下代码

``` js
var salary = "1000$";
(function() {
    var salary;
    console.log("Original salary was " + salary);
    salary = "5000$";
    console.log("My New Salary " + salary);
})();
```

</details>

<b><details><summary>208. 什么是 instanceof 操作符？下面代码输出什么？</summary></b>

``` js
function foo() {
    return foo;
}

console.log(new foo() instanceof foo);
```

instanceof操作符用来判断是否当前对象是特定类的对象。

参考答案：返回 false

``` js
function Animal() {
    //或者不写return语句
    return this;
}
var dog = new Animal();
dog instanceof Animal // Output : true
```

</details>

<b><details><summary>209. 如果我们使用JavaScript的"关联数组"，我们怎么计算"关联数组"的长度？</summary></b>

``` js
var counterArray = {
    A: 3,
    B: 4
};
counterArray["C"] = 1;
```

参考答案：其实参考答案很简单，直接计算key的数量就可以了。

``` js
Object.keys(counterArray).length // Output 3
```

</details>

<b><details><summary>210. 针对json数组，根据某一个值排序 </summary></b>

参考答案：

``` js
var arrJson = [{
        flight: "ERWIO",
        price: 350
    },
    {
        flight: "WW250",
        price: 120
    },
    {
        flight: "QQ350",
        price: 100
    },
    {
        flight: "SDJIN",
        price: 300
    },
    {
        flight: "MH370",
        price: 120
    }
];

function sortByKey(array, key) {
    return array.sort(function(a, b) {
        var x = a[key];
        var y = b[key];
        return ((x < y) ? -1 : ((x > y) ? 1 : 0));
    });
}
arrJson = sortByKey(arrJson, 'price');
```

</details>

<b><details><summary>211. 下面代码输出什么？</summary></b>

``` js
var a = {};
b = {
    key: 'b'
};
c = {
    key: 'c'
};
a[b] = 123;
a[c] = 456;
console.log(a[b]);
```

参考答案：456

首先a声明为一个对象，b和c也是对象，执行a[b] = 123时，b会转成字符串(调用toString()方法)来当作a对象的键，b.toString() === '[object Object]'，所以a对象此时是这样的  a = {'[object Object]':123}，同理可知，c也是一个对象，所以a[c] = 456执行，其实也是a['[object Object]'] = 456，把原本的123覆盖了，故输出a[b]也就是输出a['[object Object]']，输出456。

</details>

<b><details><summary>212. 下面代码输出什么？</summary></b>

``` js
(function(x){
    return (function(y){
        console.log(x);
    })(2)
})(1);
```

参考答案：1

很简单，一个自执行函数返回了一个自执行函数，所以二者都会顺利执行，外层函数1当作x传入，内层函数2当作2传入，内层函数可以访问外层函数的变量，打印的x就是1 。

</details>

<b><details><summary>213. 下面代码输出什么？</summary></b>

``` js

```

参考答案：

</details>
