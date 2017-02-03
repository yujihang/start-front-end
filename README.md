# 学习前端过程中写的一些demo

野生前端找工作中，微信：win5do，QQ：244495733

## demo lists

- vue.js问卷管理webapp <a href="http://win5do.cc/jianqn/#/" target="_blank">livedemo</a>    <a href="#demo1">项目介绍</a>
- 应用各种动画的fullpage专题页面 <a href="http://win5do.cc/xx2" target="_blank">livedemo</a>     <a href="#demo2">项目介绍</a>

<a href="http://win5do.cc/jianqn/#/" target="_blank"><h3 id="demo1">vue.js打造的问卷管理webapp</h3></a>

1. 简介

    这是根据<a href="http://ife.baidu.com/2016/task/detail?taskId=50" target="_blank">百度前端技术学院第50项任务</a>写的一个小型问卷网站，用vue.js打造成单页面web应用。可以创建、保存、填写问卷，能够统计填写结果并以表格的方式呈现出来。

2. 技术栈

- vue全家桶：vue.js + vuex + vue-router
- 数据从localstore存取
- 小图标：fonticon
- 数据图表：echarts
- 动画引入了animate.css
- 日历组件：vue-datepicker
- 工程化：vue-cli + webpack构建及打包，npm安装相关依赖；es6语法，eslint校验代码规范

3. 主要功能
- 大致结构：一个主界面，有新建问卷、问卷列表、编辑问卷、填写问卷、查看数据这五个主组件，组件通过路由跳转。一个modal弹框提示公用组件。
- 新建问卷：可以选择单选、多选、简答三种问题，问题和选项内容input里修改，通过v-model双向绑定。选择题可以添加和删除选项，问题可以删除、上下移动排序。
- 问卷列表：对已保存的问卷进行，编辑、发布、删除、查看数据等操作。问卷有已保存、已发布、已过期三种状态，根据不同的状态渲染不同的操作按钮，比如发布之后就不能编辑了。
- 编辑问卷：跟新建问卷功能相同，将之前保存的问卷数据传递编辑页二次编辑保存。
- 填写问卷：填写发布的问卷，保存时将填写的结果保存进数据中。
- 查看数据：处理填写问卷的数据，处理成echarts配置所需的data格式，用echarts生成图表，单选是饼图，多选是雷达图，简答是柱形图。
- 弹框：根据各组件中不同的场景给出相应的提示

4. 碰到的问题
- 组件间的数据传递：开始的时候没打算用vuex，因为没用过(⊙﹏⊙)b，vue文档里提到非父子组件通信的一种方法是创建一个空的vue实例作为bus，通过$emit和$on触发和监听事件，在回调中传输数据，但是两个组件是路由跳转的关系，通信失败只好考虑vuex了。看了下vuex文档大概了解下用法，就用到了项目里了，在vue的chrome dev插件里也能监控store中的数据变化。文档中提到更改 Vuex 的 store 中的状态的唯一方法是提交 mutation，但我发现我的数据都是obj，直接修改键值也是可以更改store中的状态的，但是为了更好的追踪数据动向，还是选择遵守vuex的规矩。不过编辑问卷组件中，对问题的编辑没有采用Mutation提交更改，而是直接通过input的v-model直接修改了。如果每个input的编辑都去Mutation，是不是搞复杂了？
- 处理问卷填写的结果：问卷填写完成，需要得到选中的选项，监听点击事件或者双向绑定radio或checkbox的数据似乎都不好使，这个时候就怀念起jquery的好了，点击提交直接检查checked属性就可以得到结果了。好在vue也能通过ref直接操作dom，将结果存入数组中，然后查看数据时再通过计算处理成echarts图表所需的配置项。echarts是很炫酷，不过一大堆配置项搞得头都大了。
- 日历组件：本以为github上这种组件应该有一大堆，找了半天不是vue1.0老组件的就是没有文档，终于找到一个文档写的挺全的还是中国人写的，虽然有个小bug，就是设置了起始日期但是可以选中上个月的日期。吐槽一句，看api不如自己造轮子有快感，但是写业务的话还是轮子快。
- 单机的问卷：任务需要应该大部分满足了，但是有个很尴尬的问题就是问卷虽然能填能看但是只能自己填，数据储存在本地localstore里，岂不是一个单机的问卷。于是乎想着怎么把数据弄到服务器上去，在填写也通过ajax请求数据。但是对数据库和后端的知识欠缺，只好现学现卖。在慕课上跟着sccot老师学nodejs，刚捣腾了一下fs模块，然后发现我买的阿里云虚拟主机并不能部署node，只支持php，作为一个前端门都没入的菜鸡还是先专注前端吧，卒！

5. 总结

    几个月前做IFE前面的任务时偷瞄了一下后面的任务，看到别的同学提交的用react完成的作品，感觉好炫酷好高大上，看了一下任务要求顿时让我望而却步。后来听说只会jq已经找不到工作了，开始接触react、vue，学react的时候感觉一脸懵逼，有着中文文档且更好上手的vue显得更可爱。vue教程看了一遍，尤大的几个官方demo都让我学到了不少，想找个东西练练手，于是想起了这个任务，这种频繁变更dom的场景很适合这种mvvm框架。花了三四个星期，边学变做，开始只想试试vue，最后router、vuex都用上了，感觉很炫酷的图表也借助echarts实现了，之前感觉不可能的任务终于完成，成就感满满。vue这种数据驱动的mvvm框架，除了写一些式样、模板，大部分精力放在数据和逻辑上，这种编程体验真的非常爽！尤大写出这种框架真是太厉害了，真让人自惭形秽。不过想想自己之前看一些入门的教程都觉得吃力，现在去看就觉得挺简单了，冰冻三尺非一日之寒，别总待在舒适区，每天进步一点，可能离大神还很遥远，但也比今天更好！

<a href="http://win5do.cc/xx2" target="_blank"><h3 id="demo2">一个交互丰富的专题页面</h3></a>

1. 简介

    仿写的某游戏的官网<a href="http://xx2.ztgame.com" target="_blank">xx2.ztgame.com</a>,看到这个网站的fullpage滚动，css3动画，视频弹出层，弹幕等都是当下比较热门的交互，并且有pc端和移动端。于是自己尝试写一下，用scss写的式样，根据自己的理解写的js逻辑，之原来的差别很大。

2. 技术栈

    pc端：jquery、scss、html5、css3
    移动端：zepto、rem布局、zepto.fullpage插件

3. 主要功能
- 弹幕：第一屏可开启弹幕，弹幕有三种式样，四条轨道，随机生成，速度也是随机的，后一个弹幕不会与前一个弹幕式样和轨道重复。并且可以手动发送弹幕，控制弹幕的两个按钮可以随状态变化。
- 屏幕上的花瓣：跟随移动鼠标的花瓣，可以正向或反向移动。
- 时髦的fullpage效果：滚轮和点击导航都可以控制，pc端自己写的js，移动端尝试应用下插件，采用的zepto.fullpage插件。原网页使用的是swiper，事实证明swiper效果更生动，还能用在m端的轮播里面。
- css及jquery动画：页面中动画效果非常多。

4. 碰到的问题
- 弹幕：开始看到原网站的弹幕感觉好困难，仔细研究了一番，也就随机式样，然后把设定好的弹幕数组填充进去，逻辑通了，分解成个小步实现起来也没有想象的那么难。
- 花瓣：开始想让花瓣移动的距离在一定范围内随机生成，但是mousemove频繁触发，花瓣抖的非常厉害，转整数也不行，只好用固定值了。
- 移动端滑动手势：移动端有个轮播，没用swiper插件只好自己写咯。以前没写过touch事件，网上查了一下，touchstart和touchend获取起始坐标，再来计算角度判断滑动方向，简易的实现了。以45度为界是不妥的，30度差不多，手势与运动方向相反。
- 图片懒加载：第二屏预览图片通过lazyload延迟加载，通过new Image()再指定src，以前没接触过这歌，照着写的，看network似乎是实现了lazyload，对浏览器资源请求这块不是太懂。
- 不能跨域ajax：原网站第一屏有个flash，把地址改一下链接是可以打开的，但是在我的页面上会卡住，下到本地也不行，第一屏就用了张图了，flash+弹幕也会非常卡。原网站新闻列表也是通过ajax请求的，我这边当然拿不到了，就用静态的代替了。

5. 总结

看到这个网站挺时髦就来仿写了，想着简历也用这种全屏滚动的方式。以前都没有写过这么多js，光pc端就有四五百行，开始是从上到下的写法，才实现了一小部分就发现这么写代码太乱了，于是改成对象写法，相互调用的时候踩了很多this和作用域的坑，不得不重温下js红宝书。通过这个项目加深了自己对动画，事件，作用域的理解以及this的理解，对jq和css3的运用也更熟练了。但是即便是用了对象写法，而且注释也写得蛮多，但是fix bug的时候上下翻着看代码的时候还是感觉蛮蛋疼，很多地方要是封装成函数，通过传参、回调来调用，逻辑应该会更清晰了，解耦这方面还差很远。写过稍微复杂一点的项目才明白，模块化的必要性，webpack大法好！