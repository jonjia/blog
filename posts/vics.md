


![vue.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/095c0cbf0e9b42a184a3fc322f625631~tplv-k3u1fbpfcp-watermark.image)

**vue 项目自动国际化命令行工具 [vue-i18n-code-shift](https://github.com/jonjia/vue-i18n-code-shift)，欢迎使用和反馈，也可以帮忙点个 star**

## 背景

前端项目国际化过程中，一项任务就是把中文字符串字面量替换成语料资源的引用 `$t('common.text')`。对于大型项目（笔者的项目中需要操作 400 个文件、12000 行的代码量），依靠手动操作不仅费时费力还容易出错。如果有一个自动国际化的工具就好了？

## 思路

#### 项目规划

1. 实现⼀个⾃动化的前端国际化全流程解决⽅案
2. 开源出来，提供给其他项目使用

#### 方案规划

- 依靠正则匹配：由于代码灵活性带来的复杂度和可维护性问题，**正则方案很快被放弃**
- 先将源码转为 AST，再进行分析和转换
  1. 梳理自动国际化⼯具需要提供的功能（提取、替换、导出语料、导入语料等）
    ![工具做什么.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af0bc1e0e7824131ace71597bdc4889b~tplv-k3u1fbpfcp-watermark.image)
  2. 调研业界国际化⼯具实现思路

## 实现

1. 主要处理 .vue 和 .js 文件，.vue 中的样式代码一般不涉及中文字符串字面量，直接忽略 `<style></style>` 部分
2. 使用 [vue-template-complier](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#readme) 解析 .vue 文件，使用 ts 将 JS 代码解析成 AST，[vue-template-complier](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#readme) 也提供了将 vue 模版代码解析成 AST 的方法
3. 使用 `String.prototype.replace()` 直接操作源码替换，**说好的 AST 呢**
4. 导入、导出就是处理语料资源，再使用 [node-xlsx](https://www.npmjs.com/package/node-xlsx) 的构建和导出 api
5. 其他是一些命令行工具通用的包

## 重构

#### 原因

- 由于上面第 4 点中使用的 `String.prototype.replace()`，代码既复杂又不好维护，就想找一个更优的方案
- 对于一些不太常见的语法，提取和替换有一定问题，比如

```
['失效', '生效'][+scope.row.index]
`模版${a}字符串`
```

#### 结果

- 尝试使用 [jscodeshift](https://github.com/facebook/jscodeshift) 来重构提取、替换逻辑，**提取实现难度不大，但要包括不同的语句类型。替换的实现中遇到阻碍，模版字符串还是无法处理（主要和他的 AST 结构有关），替换要处理的语句类型也很复杂，比之前的字符串替换并没有改变**。放弃
- 尝试使用 [vue-eslint-parser](https://github.com/vuejs/vue-eslint-parser) 代替来 [vue-template-complier](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#readme) 解析 vue 模版，只是为了解决某些语法识别的问题。`['失效', '生效'][+scope.row.index]` 这个问题好了，但又带来了原来没有的问题。后来考虑即使成功了也没有本质性变化，也就放弃了

## 思考

1. 开始重构代码之前一定有一个验证过程，不然很容易过于自信
2. 知识域很重要，不然只会在低级的解决方案上浪费工夫，所以平时要多了解、多积累
