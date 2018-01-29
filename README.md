localStorage使用总结
一、什么是localStorage、sessionStorage

在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。

 

二、localStorage的优势与局限

localStorage的优势

1、localStorage拓展了cookie的4K限制

2、localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的

localStorage的局限

1、浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性

2、目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换

3、localStorage在浏览器的隐私模式下面是不可读取的

4、localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡

5、localStorage不能被爬虫抓取到

localStorage与sessionStorage的唯一一点区别就是localStorage属于永久性存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空

这里我们以localStorage来分析

 

三、localStorage的使用

localStorage的浏览器支持情况：



这里要特别声明一下，如果是使用IE浏览器的话，那么就要UserData来作为存储，这里主要讲解的是localStorage的内容，所以userData不做过多的解释，而且以博主个人的看法，也是没有必要去学习UserData的使用来的，因为目前的IE6/IE7属于淘汰的位置上，而且在如今的很多页面开发都会涉及到HTML5\CSS3等新兴的技术，所以在使用上面一般我们不会去对其进行兼容

首先在使用localStorage的时候，我们需要判断浏览器是否支持localStorage这个属性

if(！window.localStorage){
            alert("浏览器支持localstorage");
            return false;
        }else{
            //主逻辑业务
        }

 

localStorage的写入，localStorage的写入有三种方法，这里就一一介绍一下

if(！window.localStorage){
            alert("浏览器支持localstorage");
            return false;
        }else{
            var storage=window.localStorage;
            //写入a字段
            storage["a"]=1;
            //写入b字段
            storage.a=1;
            //写入c字段
            storage.setItem("c",3);
            console.log(typeof storage["a"]);
            console.log(typeof storage["b"]);
            console.log(typeof storage["c"]);
        }

 

运行后的结果如下：



这里要特别说明一下localStorage的使用也是遵循同源策略的，所以不同的网站直接是不能共用相同的localStorage

最后在控制台上面打印出来的结果是:



不知道各位读者有没有注意到，刚刚存储进去的是int类型，但是打印出来却是string类型，这个与localStorage本身的特点有关，localStorage只支持string类型的存储。

localStorage的读取


if(!window.localStorage){
            alert("浏览器支持localstorage");
        }else{
            var storage=window.localStorage;
            //写入a字段
            storage["a"]=1;
            //写入b字段
            storage.a=1;
            //写入c字段
            storage.setItem("c",3);
            console.log(typeof storage["a"]);
            console.log(typeof storage["b"]);
            console.log(typeof storage["c"]);
            //第一种方法读取
            var a=storage.a;
            console.log(a);
            //第二种方法读取
            var b=storage["b"];
            console.log(b);
            //第三种方法读取
            var c=storage.getItem("c");
            console.log(c);
        }

 

这里面是三种对localStorage的读取，其中官方推荐的是getItem\setItem这两种方法对其进行存取，不要问我这个为什么，因为这个我也不知道

我之前说过localStorage就是相当于一个前端的数据库的东西，数据库主要是增删查改这四个步骤，这里的读取和写入就相当于增、查的这两个步骤

下面我们就来说一说localStorage的删、改这两个步骤

改这个步骤比较好理解，思路跟重新更改全局变量的值一样，这里我们就以一个为例来简单的说明一下

 


if(!window.localStorage){
            alert("浏览器支持localstorage");
        }else{
            var storage=window.localStorage;
            //写入a字段
            storage["a"]=1;
            //写入b字段
            storage.b=1;
            //写入c字段
            storage.setItem("c",3);
            console.log(storage.a);
            // console.log(typeof storage["a"]);
            // console.log(typeof storage["b"]);
            // console.log(typeof storage["c"]);
            /*分割线*/
            storage.a=4;
            console.log(storage.a);
        }

 

这个在控制台上面我们就可以看到已经a键已经被更改为4了

localStorage的删除

1、将localStorage的所有内容清除


var storage=window.localStorage;
            storage.a=1;
            storage.setItem("c",3);
            console.log(storage);
            storage.clear();

 

2、 将localStorage中的某个键值对删除

 


var storage=window.localStorage;
            storage.a=1;
            storage.setItem("c",3);
            console.log(storage);
            storage.removeItem("a");
            console.log(storage.a);

 

控制台查看结果



localStorage的键获取


var storage=window.localStorage;
            storage.a=1;
            storage.setItem("c",3);
            for(var i=0;i<storage.length;i++){
                var key=storage.key(i);
                console.log(key);
            }

 

使用key()方法，向其中出入索引即可获取对应的键

 

四、localStorage其他注意事项

 一般我们会将JSON存入localStorage中，但是在localStorage会自动将localStorage转换成为字符串形式

这个时候我们可以使用JSON.stringify()这个方法，来将JSON转换成为JSON字符串


if(!window.localStorage){
            alert("浏览器支持localstorage");
        }else{
            var storage=window.localStorage;
            var data={
                name:'xiecanyong',
                sex:'man',
                hobby:'program'
            };
            var d=JSON.stringify(data);
            storage.setItem("data",d);
            console.log(storage.data);
        }


读取之后要将JSON字符串转换成为JSON对象，使用JSON.parse()方法

var storage=window.localStorage;
            var data={
                name:'xiecanyong',
                sex:'man',
                hobby:'program'
            };
            var d=JSON.stringify(data);
            storage.setItem("data",d);
            //将JSON字符串转换成为JSON对象输出
            var json=storage.getItem("data");
            var jsonObj=JSON.parse(json);
            console.log(typeof jsonObj);
