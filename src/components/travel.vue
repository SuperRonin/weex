<template>
        <scroller class="scroller" show-scrollbar="false">
            <refresh class="refresh" @refresh="onrefresh" @pullingdown="onpullingdown" :display="refreshing ? 'show' : 'hide'">
                <loading-indicator class="indicator"></loading-indicator>
                <text class="indicator-text">{{ refreshTxt }}</text>
            </refresh>
            <div class="cell" v-for="item in travelList">
                <div class="scenic-item" @click="onItemClick">
                <div class="sce-img-box">
                        <image class="sce-img" src="/web/assets/img/travel_mock.png" alt=""></image>
                    </div>
                    <div class="sce-info">
                        <text class="sce-intr" line="2">{{ item.name }}</text>
                        <image src="" alt=""></image><text class="pt color-gray">出发城市：{{ item.startcity }}</text>
                        <image src="" alt=""></image><text class="pb color-gray">目的城市：{{ item.endcity }}</text>
                        <div class="price">
                            <text>¥ </text>
                            <text class="color-red">{{ item.price }}</text>
                            <text>起/每人</text>
                        </div>
                    </div>
                </div>
            </div>
            <loading class="loading" @loading="onloading" :display="loadinging ? 'show' : 'hide'">
                <loading-indicator class="indicator"></loading-indicator>
                <text class="indicator-text">{{loadTxt}}</text>
            </loading> 
        </scroller>
</template>
<style scoped>
    .pt{
        padding-top: 12px;
    }
    .pb{
        padding-bottom: 12px;
    }
    .color-gray{
        font-size: 25px;
        color: #555;
    }
    .color-red{
        font-size: 50px;
        color: red;
    }
    .scenic-item{
        width: 750px;
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        justify-content: space-around;
        padding: 0 20px;
        box-sizing: border-box;
        margin: 15px 0;
    }
    .sce-img-box{
        width: 300px;
        padding: 10px;
        box-sizing: border-box;
    }
    .sce-img{
        width: 270px;
        height: 240px;
        border-radius: 10px;
    }
    .sce-info{
        width: 450px;
        padding:10px;
        box-sizing: border-box;
    }
    .sce-intr{
        font-weight: bold;
        height: 88px;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
    }
    .price{
        flex-direction: row;
        flex-wrap: nowrap;
        justify-content: flex-start;
        align-items: flex-end;
    }
    .refresh{
        width: 750;
        display: -ms-flex;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        justify-content: center;
        -webkit-align-items: flex-end;
        align-items: flex-end;
    }
    .loading {
        width: 750;
        display: -ms-flex;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .indicator-text {
        color: #888888;
        font-size: 30px;
        text-align: center;
        margin-left: 10px;
    }
    .indicator {
        height: 20px;
        width: 40px;
        color: #999;
    }
</style>

<script>
    const modal =  weex.requireModule('modal');
    const navigator = weex.requireModule('navigator');
    let nextUrl = 'http://192.168.2.108:8081/src/components/load.vue'
    export default {
        name: 'travel',
        data () {
            return {
               baseURL : '',
               loadinging: false,
               refreshing: false,
               refreshTxt: '松手加载更多~~~',
               loadTxt: '上拉加载更多',
               travelList: [
                   {
                       'name': '北京5日当地游五钻帝都，三环五星酒店，颐和园品皇颐和园品皇品皇',
                       'price': 25,
                       'startcity': '九江',
                       'endcity': '昌平'
                   },
                   {
                       'name': '北京5日当地游五钻帝都，三环五星酒店，颐和园品皇颐和园品皇品皇',
                       'price': 24,
                       'startcity': '九江',
                       'endcity': '昌平'
                   },
                   {
                       'name': '北京5日当地游五钻帝都，三环五星酒店，颐和园品皇颐和园品皇品皇',
                       'price': 288,
                       'startcity': '九江',
                       'endcity': '昌平'
                   },
                   {
                       'name': '北京5日当地游五钻帝都，三环五星酒店，颐和园品皇颐和园品皇品皇',
                       'price': 22,
                       'startcity': '九江',
                       'endcity': '昌平'
                   },
                   {
                       'name': '北京5日当地游五钻帝都，三环五星酒店，颐和园品皇颐和园品皇品皇',
                       'price': 25,
                       'startcity': '九江',
                       'endcity': '昌平'
                   }
               ]
            }
        },
        beforeCreate (){

        },
        created () {
            
        },
        methods: {
            translateRule: function (dp) {
                return dp * 750 / ( deviceWidth / scale );
            },
            onrefresh (event) {
                // modal.toast({ message: '正在获取数据~~~', duration: 0.5 })
                
                this.refreshing = true
                setTimeout(() => {
                    this.refreshing = false
                    // this.refreshTxt = '刷新成功'
                },1000)
            },
            onpullingdown (event) {
                // setTimeout(() => {
                //     modal.toast({ message: '这里可以加入动画~~~', duration: 0.5 })
                // },500)
            },
            onloading () {
                this.loadinging = true
                setTimeout(() => {
                    this.loadinging = false
                }, 2000)
            },
            onItemClick:function(e){
                var params = {'url':nextUrl,'animated':'true'}
                navigator.push(params, function(e) {
                    console.log('i am the callback.')
                });
            }

        }
    }
</script>