# 模块异步加载

为了提高单据加载速度，我们会把非必要性单据功能进行独立打包，异步加载。

目前侧边栏的功能都是模块化异步加载的。

目前公共的模块加载器都定义在 `/src/components/voucher/controller/ModuleController.ts`  文件中。

我们的项目基于 webpack 进行打包，所以关于打包拆包的细节问题可以参考： [Code Splitting](https://webpack.github.io/docs/code-splitting.html)

