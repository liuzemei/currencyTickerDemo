本文旨在研究比特币量化交易。无意中发现，其实一款简易的行情软件可能5真的只要5分钟...就留下这篇教程，希望让更多的人了解到相关的开发。抛砖引玉~
> 技术栈： html css js vue elementUI ccxt

## 流程
1. 行情系统无非就是一张动态数据的表格。
2. 只要获取到数据，然后按照一定的方式展示就行了。

### 展示
> 
```html
<!DOCTYPE html><!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <script src="https://unpkg.com/element-ui/lib/index.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/ccxt@1.18.1146/dist/ccxt.browser.js"></script>
</head>

<body>
    <div id="app">
        <h1 style="text-align: center">5分钟开发一个行情软件</h1>
        <el-table :data="tableData" style="width: 100%">
            <el-table-column prop="info.id" width="50" sortable></el-table-column>
            <el-table-column prop="symbol" label="交易对" sortable></el-table-column>
            <el-table-column prop="info.high24hr" label="24小时最高价" sortable></el-table-column>
            <el-table-column prop="info.low24hr" label="24小时最低价" sortable></el-table-column>
            <el-table-column prop="info.last" label="最新价格" sortable></el-table-column>
            <el-table-column prop="info.highestBid" label="当前最高价买价" sortable></el-table-column>
            <el-table-column prop="info.lowestAsk" label="当前最低价卖价" sortable></el-table-column>
            <el-table-column prop="info.percentChange" label="涨跌幅" sortable></el-table-column>
        </el-table>
    </div>
    <script>
        var app = new Vue({ // Vue实例 动态绑定数据
            el: '#app',
            data() {
                return {
                    tableData: [],
                    timer: null
                }
            },
            mounted() {
                updateDataTimer.call(this);
            },
            destroyed() {
                clearInterval(this.timer);
                this.timer = null;
            }
        });
        function updateDataTimer() {  // 设计一个定时器，不停的去更新数据
            this.timer = setInterval(() => {
                getExchangeInfo.call(this);
            }, 1000);
        }
        async function getExchangeInfo() { // 获取交易所的数据
            const exchange = new ccxt['poloniex']({
                enableRateLimit: true // 打开交易限速
            });
            this.tableData = await exchange.fetch_markets({}); // 将值传递到Vue的实例中
        }
    </script>
</body>

</html>
```