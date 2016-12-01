//       核心原型
           itcast.fn= jquery.prototype=function(){
               constructor:itcast
           }
//       构造函数

       init=jquery.fn.init=function(selector){}
       //          实现init对象继承自jquery的原型
//         init创建出来的对象，最终继承自itcast.prototype。
//          所以可以将init对象称为 jquery对象。
       init.prototype=jquery.fn;


//       可扩展方法
//       如果target为undefined值，那么就是给this扩展成员
//            否则就是给target对象扩展
      jquery.extend=jquery.fn.extend=function(source,target){
          var k;
           if(target===undefined){
               target=this;
           }
          for(k in source){
              target[k]=source[k];
          }
       }
//    暴露给用户
    global.$=global.jquery=jquery;
   }(window))		

```

分析如下：  
  1. jquery在编写框架的时候，使用了沙箱模式   
   + 在沙箱内部，如果经常使用全局变量或全局对象的话，最好的做法就是将它们当做实参传入沙箱内。

	//因为这样子每次你去访问全局变量的时候，先去自己的作用域去找，如果找不到就去上一级
	寻找，直到找到全局作用域，因为你本来知道就是全局，所以为了优化性能，避免每次都要去查找全局作用域，所以会把全局作用域当做实参传进沙箱内）

	2. 核心函数jQuery。最终要暴露给用户使用的

3. 实现itcast函数，使用的是 工厂模式 来 创建对象。好处：用户 new 或 不 new 都可以得到正确的对象

4. init构造函数 的 位置

	+ 如果放在沙箱内部，用户是无法修改或重写的。所以要容纳更改用户，尽量将构造函数暴露给用户

	+ 可以把构造函数放在jquery函数上，也可以放在jquery函数原型上。

	+ 处于jQuery之父，在写简单继承模式时，将构造函数放在其原型上。那么在编写框架时，即延续下

5. init创建出来的对象，最终继承自itcast.prototype。所以可以将init对象称为 itcast对象。

6. 由于暴露给用户 的 是 jquery 和 其原型。所以在扩展成员时，只能在这两个对象上扩展。
	而在函数对象上扩展的成员 为 静态成员。可以直接通过函数名字来访问。但是，在原型上的成员，
	必须创建实例来访问。因此为了实现init对象可以访问 jquery原型上的成员，就基于原型来实现继承。