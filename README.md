# 估值精灵

使用估值精灵，可以实时查看您关注的股票、基金的估值。

## 功能

- 支持标准与暗色两种主题

- 支持设置持有份额，查看当日估值收益

- 支持设置成本价，查看当日估值收益

- 股票基金按照指定项排序功能

- 添加股票基金时支持按拼音、汉字、编码模糊搜索，支持批量添加

- 行情中心展示：大盘资金、行业板块、北向资金、南向资金

## 待实现功能

- 实时角标提醒

- 单个股票、基金特别关注

- 角标内容自定义，单个与全部股票，收益额与收益率

- 股票列表与配置信息支持导入与导出

- 智能判断节假日，切换休市状态

- 支持手动加仓减仓功能

- 支持账号登录功能，方便跨平台同步

- 交易日更新当日实际净值后，对应股票有 √ 提示

- 支持查看估值走势图，净值、收益等走势图

- 支持支持查看股票的股票持仓明细

- 支持查看股票基本信息与股票经理信息

- 自定义顶部指数内容

- 自定义股票列表展示内容

## 框架介绍

基于[Vue](https://github.com/vuejs/vue) + [Webpack](https://github.com/webpack/webpack)构建的 vue 项目，使用了[vue-web-extension](https://github.com/Kocal/vue-web-extension/tree/v1)模板快速构建Chrome扩展项目，用到了[Element UI](https://github.com/ElemeFE/element)样式库与[ECharts](https://github.com/apache/echarts)图表库。

## 运行调试开发

需要 node 环境，先执行
`npm i`
安装依赖

调试模式执行
`npm run watch:dev`
生成 dist 文件夹，浏览器选择“加载已解压的扩展程序”

打包与发布先执行
`npm run build`
生成 dist 文件夹，再执行
`npm run build-zip`
通过从 manifest.json 文件中读取 name 和 version 字段,构建{name}-v{version}.zip 这种格式的压缩文件。

## 更新说明

### v1.0.0

- 支持股票和基金的添加，查看实时估值