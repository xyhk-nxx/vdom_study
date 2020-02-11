<!--
 * @Author: your name
 * @Date: 2020-02-08 17:41:36
 * @LastEditTime: 2020-02-08 18:12:49
 * @LastEditors: your name
 * @Description: In User Settings Edit
 * @FilePath: \vue-study\虚拟dom\vue_dom\readme.md
 -->
vue中的虚拟dom
vue1 响应式，Object.defineproperty 每个数据修改都要通知dom 去修改
响应式级别修改了，watcher之到组件级，组件内部使用虚拟dom

vue中虚拟dom做了那些优化
1.vue中虚拟dom 如何创建

    templete div各种标签会编译或新建成虚拟dom函数 
    compile模块解析成render函数

2.vue虚拟dom如何diff
    新老子元素都是数组的时候
    编译原理
    AST
    <div>
        {{name}}
    </div>
    解析成：
    {
        tag:'div',
        props:{},
        children:[name]
    }

    entry-runtime-with-compiler.js====>compileToFUnctions() 将模版解析成render函数
    ——》index.js =>createCompiler()
    --》index.js =>createCompilerCreator()
        const ast = parse()  parse函数 解析模块 将template解析成语法树
        optimize() 优化 比如静态节点标记
        const code = generate() 代码生成 将ast 生成可执行的代码
    <div v-if="isShow"> 哈哈</div>
    <div v-else>呵呵</div>
    return isShow?_c('div',{children:['哈哈']}):_c('div',{children:['呵呵']})


    updateChildren()
    新老都是数组，由于有些元素可能只是位置变了
    vue针对web业务的特点，在数组diff的时候，做了几个小优化
    web数组常见的修改
    1.新增元素
    2.删除元素
    3.倒序排序雨歌元素
    所以每次遍历，都会猜测一下：
    1.老的数组排头，新的数组排头，如果他俩一样，直接进行更新元素的操作逻辑，而不是用数组diff这个
    元素
    2.中间新增一个元素
    3.老的数组排尾，新的数组排尾，如果一样
    4.老的数字排头，新的数组排尾
    5.老的数组排尾，新的数字排头
    猜测对比，如果猜中，缩小遍历范围
    上面都没有猜中，那就只能走遍历的逻辑
    domdiff最核心的逻辑vm._update执行的diff



