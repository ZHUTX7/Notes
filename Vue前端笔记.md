1. ### 进入到项目目录

2. ### CMD : vi ui

3. ### 进入项目或者创建项目

### 4.axios异步发送

安装axios 安装json-server cnpm install json-server -g

### 5.Vue配置全局变量     main.js


页面可以调用端口，Postman不行，代理问题，关闭代理或者修改代理配置



6. ### Cannot destructure property xxx of 'undefined'.

比如crypto没有定义；  首先需要在main.js   import crypto from 'crypto' 引入
然后在下方Vue.prototype.$crypto = crypto
代码中引用 this.$crypto

### 7.只需要在项目的根目录下新建 vue.config.js 文件（是根目录，不是src目录）

const path = require('path')

module.exports = {
    publicPath: './', // 基本路径
    outputDir: 'dist', // 输出文件目录
    lintOnSave: false, // eslint-loader 是否在保存的时候检查

    // webpack配置
    chainWebpack: (config) => {
    },
    configureWebpack: (config) => {
        if (process.env.NODE_ENV === 'production') {
            // 为生产环境修改配置...
            config.mode = 'production'
        } else {
            // 为开发环境修改配置...
            config.mode = 'development'
        }
        Object.assign(config, {
            // 开发生产共同配置
            resolve: {
                // 别名配置
                alias: {
                    '@': path.resolve(__dirname, './src'),
                    '@c': path.resolve(__dirname, './src/components'),
                    '@p': path.resolve(__dirname, './src/pages')
                }
            }
        })
    },
    productionSourceMap: false, // 生产环境是否生成 sourceMap 文件
    // css相关配置
    // css: {
    //     extract: true, // 是否使用css分离插件 ExtractTextPlugin
    //     sourceMap: false, // 开启 CSS source maps?
    //     loaderOptions: {
    //         css: {}, // 这里的选项会传递给 css-loader
    //         postcss: {} // 这里的选项会传递给 postcss-loader
    //     }, // css预设器配置项 详见https://cli.vuejs.org/zh/config/#css-loaderoptions
    //     modules: false // 启用 CSS modules for all css / pre-processor files.
    // },
    parallel: require('os').cpus().length > 1, // 是否为 Babel 或 TypeScript 使用 thread-loader。该选项在系统的 CPU 有多于一个内核时自动启用，仅作用于生产构建。
    pwa: {}, // PWA 插件相关配置 see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
    // webpack-dev-server 相关配置
    devServer: {
        open: process.platform === 'darwin',
        host: '0.0.0.0', // 允许外部ip访问
        port: 8088, // 端口
        https: false, // 是否启用https
        // 错误、警告在页面弹出
        overlay: {
            warnings: true,
            errors: true
        },
        // 代理转发配置，用于调试环境
        proxy: {
            '/api': {
                target: 'http://192.168.0.111:4000',   //目标地址
                changeOrigin: true, // 允许websockets跨域
                // ws: true,
                pathRewrite: {
                    '^/api': ''
                }
            }
        }
    },
    // 第三方插件配置
    pluginOptions: {}
}



### 8.解决跨域问题

### 8.1配置代理

​        proxy: {
​            '/api': {
​                target: 'http://192.168.0.111:4000',   //目标地址
​                changeOrigin: true, // 允许websockets跨域
​                // ws: true,
​                pathRewrite: {
​                    '^/api': ''
​                }
​            }
​        }

​	

### 9.接受token保存在cookies中

```
//在main.js中导入cookies
import VueCookies from 'vue-cookies'
Vue.use(VueCookies)
//.axios发送登录请求后将token保留到
 const _this = this;
_this.$cookies.set('Access-Token',resp.data.token);
```



--------------------------
ElementUI Table表格 可选 全选



### 10.Vue调用Fomater

```
<el-table-column
        prop="time"
        label="时间"
        sortable
        width="150"
:formatter="formatter">
```

### 11.时间戳转换日期

```
   formatter(timeTamp){
    var date = new Date( timeTamp * 1000);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
    var Y = date.getFullYear() + '-';
    var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
    var D = date.getDate() + ' ';
    var h = date.getHours() + ':';
    var m = date.getMinutes() + ':';
    var s = date.getSeconds();
    return Y + M + D + h + m + s;
    }

```



### 12. 分页

```
<div class="block">
    <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :page-sizes="[5, 10, 30, 40]"
            :page-size="5"
            layout="total, sizes, prev, pager, next, jumper"
            :total=tableData.length>
    </el-pagination>
</div>
```

```javascript
<template>
    <div style="height: 100%">
        <el-table
                :data="tableData"
                style="width: 100%"
                :default-sort = "{prop: 'date', order: 'descending'}"
        >
            <el-table-column
                    prop="time"
                    label="时间"
                    sortable
                    width="150"
            :formatter="formatter">
            </el-table-column>
            <el-table-column
                    prop="ip"
                    label="ip"
                    sortable
                    width="150">
            </el-table-column>
            <el-table-column
                    prop="username"
                    label="用户"
                    sortable
                    >
            </el-table-column>
            <el-table-column
                    prop="op_desc"
                    label="操作类型"
                    sortable
                    >
            </el-table-column>
            <el-table-column
                    prop="result"
                    label="结果"
                    sortable
                    >
            </el-table-column>
            <el-table-column
                    prop="desc"
                    label="描述"
                    sortable  width="350"
                    >
            </el-table-column>
        </el-table>
        <div class="block">
            <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :page-sizes="[5, 10, 30, 40]"
                    :page-size="5"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total=tableData.length>
            </el-pagination>
        </div>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                pageInfo :
                    {
                    "Access-token" :this.$cookies.get('srstoken'),
                    limit : '',
                    keyword: "",
                    limit: 5,
                    page: 1,
                    start_time: "",
                    },
                tableData: [
                    {
                        time: '2016-05-02',
                        ip:'192.168.0.1',
                        username: '王小虎',
                        result:'ok',
                        op_desc: '登录系统',
                        op_type: 0,
                        desc: '111'
                     },
                 ],

            }
        },
        mounted:function() {
            this.GetSysAudit();
        },
        methods: {
            getTotal(){
                return this.total;
            },
            //修改每页显示条数
            handleSizeChange(val) {

                this.pageInfo.limit = val;
                this.GetSysAudit();
                console.log(`每页 ${val} 条`);
            },
            //切页
            handleCurrentChange(val) {
                this.pageInfo.page = val;
                this.GetSysAudit();
                console.log(`当前页: ${val}`);
            },
            //将时间戳转换成时间
            formatter(row, column) {
                var date = new Date(row.time * 1000);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
                var Y = date.getFullYear() + '-';
                var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
                var D = date.getDate() + ' ';
                var h = date.getHours() + ':';
                var m = date.getMinutes() + ':';
                var s = date.getSeconds();
                return Y + M + D + h + m + s;
            },
            GetSysAudit(){
                let _this = this;
                _this.Access_token =  _this.$cookies.get('srstoken');
                this.axios.post('/api/ajax/asset/getSysAudit', this.pageInfo).then(function (resp) {
                    //1.与后端服务器建立连接
                    if (resp.data.errorCode == 0) {
                        //2.后端返回登录成功信息
                        _this.tableData  = resp.data.sysauditlist;
                        _this.tableData.length = resp.data.total;
                        //测试反馈出来的日志总条数
                        //alert(_this.currentTotal);
                    }
                    else{
                        //3.后端返回登录失败信息,返回登录页面
                        alert("登陆超时，请重新登陆.");
                        _this.$router.push('/');
                    }

                }).catch(function (error){
                    alert(error);
                });
            }
        }
    }
</script>

<style scoped>

</style>
```



### 13.在表格信息中修改某行信息

```
关键：(scope.$index, scope.row)

 <el-table-column label="修改信息">
                    <template slot-scope="scope">
                        <el-button
                                size="mini"
                                @click="mTest(scope.$index, scope.row)">Edit</el-button>
                        <el-button
                                size="mini"
                                @click="dialogFormVisible=true">2</el-button>
                    </template>

                </el-table-column>
```

scope.$index可以获取下标值，下标从0开始计算

 scope.row可以获得该行所有数据，返回的是一个tabledata[index]的一个对象，比如有一列为name sex pwd,

想获取某选中行的name可以 通过scope.row.name获得



### 14.获取用户选中行

```
//1. **************ref="multipleTable"**********
<el-table :data="tableData" border style="width: 100%" ref="multipleTable">
    <el-table-column type="selection" width="40" align="center"></el-table-column>                        
    <el-table-column type="index" label="序号" width="50" :index="indexMethod" align="center"></el-table-column>
    <el-table-column prop="displayName" label="中文名称" min-width="110" align="center"></el-table-column>
    <el-table-column prop="name" label="英文名称" min-width="110" align="center"></el-table-column>
</el-table>
```





### 15.Vue Post下载

```
//1.发送post请求
downloadFile(){

    let _this =this;
    this.axios({
        method: 'post',
        url: '/api/ajax/download/backup',
        data: {
            "Access-Token":this.$cookies.get("Access-Token")
        },
        responseType: 'blob'
    }).then(response => {
        this.download(response.data)
    }).catch((error) => {

    })

},
//2.下载方法
download (data) {
    if (!data) {
        return
    }
    let url = window.URL.createObjectURL(new Blob([data]))
    let link = document.createElement('a')
    link.style.display = 'none'
    link.href = url
    link.setAttribute('download', 'backup.sql')

    document.body.appendChild(link)
    link.click()
}
```