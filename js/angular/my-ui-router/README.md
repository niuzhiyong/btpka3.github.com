
## ui-router 相关命名规则

* 状态命名规则: `main.${stateLvl1}.${stateLvl2}.${stateLvl3}`
* URL命名规则: `/${stateLvl1}/${stateLvl2}/${stateLvl3}`
* 状态js文件的命名规则 : `src/controllers/${stateLvl1}.js`. 也就是说,只要一级状态一致的, 都放到一个JS文件中. 示例：

    ```
    // 即使有嵌套的状态 user.phone.update， 也全都写在 user.js 中
    src/controllers/user.js
    ```


* ui-view ： 类似与JavaScript变量的命名规则，比如 "userDetail"， 但不允许使用 '$', 不要使用减号

* views (html文件) : `src/views/${stateLvl1}//${stateLvl2}/${stateLvl3}/${uiViewName}.${stateName}.html `。

    * 如果是匿名的ui-view，则 uiViewName = "index";
    * 如果状态是根状态，则 stateName == "root"

## 举例

期待的最终效果

```html
<!-- 为了说明演示效果, 最底层的div节点的内容会是其id上的数值 -->
<html>
<body>
    <div id="id_1" ui-view>                     <!--显示的主要内容,不同state下会内容不同 -->
        <div id="id_3" ui-view></div>               <!-- 特定状态(user)显示的内容和结构 -->
        <div id="id_4" ui-view="view4"></div>       <!-- 特定状态(user)显示的内容和结构 -->
    </div>
    <div id="id_2" ui-view="view2"></div>       <!--footer/header, 结构样式大体一致,但不同状态之间可能会略微有些差异 -->
</body>
</html>
```


然后我们把画面分解为一下两个子页面：

## src/index.html

```html
<html>
<body>
    <div id="id_1" ui-view></div>
    <div id="id_2" ui-view="view2"></div>
</body>
</html>
```

## src/views/user/index.root.html


```html
<div id="id_3" ui-view></div>
<div id="id_4" ui-view="view4"></div>
```
## src/views/user/view2.root.html

```
222
```

## src/views/user/index.user.html

```
333
```

## src/views/user/view4.user.html

```
444
```

则 定义一个状态 user， `src/controller/user.js` 内容如下：

```js
app.config([
    '$stateProvider',
    function ($stateProvider) {

        $stateProvider.state("user", {
            url: "/user",

            views: {
                "@": {
                    // 内容会填充到 id_1 下
                    templateUrl: 'views/user/index.root.html',
                    controller: ['$scope', '$http', function ($scope, $http) {
                    }]
                },
                'view2@': {
                    // 内容会填充到 id_2 下
                    templateUrl: 'views/user/view2.root.html',
                },
                '@user': {
                    // 内容会填充到 id_3 下
                    templateUrl: 'views/user/index.user.html',
                },
                'view4@user': {
                    // 内容会填充到 id_4 下
                    templateUrl: 'views/user/view4.user.html',
                }

            }
        });
    }]);
```



## 目录结构

```
src/controllers/${stateLvl1}.${stateLvl2}.${stateLvl3}.js
src/views/${stateLvl1}/index.root.html
src/views/${stateLvl1}/${uiViewName}.${stateName}.html
```
