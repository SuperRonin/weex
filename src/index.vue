<template>
  <div>
    <div class="stadions">
      <div class="find_top">
        <image class="find_top_bg" src="/dist/img/find_top_bg.png"></image>

        <div class="find_top_weather">
          <div class="current_city">
            <text id="cityname"></text>
            <image src="find/images/location_icon.png" />
          </div>
          <div class="current_temp">
            <text class="current_temperature"></text>
          </div>
          <div class="current_weather">
          </div>
        </div>
      </div>
      <!--汽车票、火车票、机票入口-->
      <div class="otherTicket" >
        <div class="item" v-for="(item,index) in otherTickets">
          <image class="otherTicket-icon" :src=item.imgLink alt=""></image>
          <text>{{ item.name }}</text>  
        </div>  
      </div>
      <div class="hidebox"></div>
      
      <!--客运站信息、国内旅游、定制客运切换-->
      <div class="find_tabs">
        <text @click="clickTab(index)"  v-for="(item,index) in findTabs" class="find-tab-items" :class="[curTabIndex == index ? 'active': '']" :index=index>{{item}}</text>
      </div>

      
        <!-- 客运站信息 -->
        <div class="stadion-info" v-if="isShow == 'stadion'">
          <scroller class="scroller" show-scrollbar="false">
            <refresh class="refresh" @refresh="onrefresh" @pullingdown="onpullingdown" :display="refreshing ? 'show' : 'hide'">
              <text class="indicator">Refreshing ...</text>
            </refresh>
            <div class="tabs_cont station_info" v-for="(item,index) in stations">
                <text class="station" lines="1">{{ item.startstationname }}</text>
            </div> 
          </scroller>  
        </div>  
      </div> 
    

    <!-- 国内旅游 -->
    <Travel v-if="isShow == 'travel'"></Travel>
    <!-- 定制客运 -->
    <Specia v-if="isShow == 'specia'"></Specia>
  </div>
</template>


<style scoped>
.scroller {
    height: 600px;
  }
body {
  background-color: #ccc;
}
.find_top {
  height: 300px;
}
.find_top_bg {
  height: 300px;
}
.otherTicket {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-around;
  padding: 20px 0;
}
.otherTicket-icon {
  height: 90px;
  margin-bottom: 10px;
  border-radius: 50%;
}
.hidebox {
  height: 20px;
  background-color: #eee;
}
.find-tab-items {
  padding: 10px;
}
.find_tabs {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-around;
  border-bottom-width: 1px;
  border-bottom-color: #9e9e9e;
  border-bottom-style: solid;
  padding: 20px 0;
}

.active {
  color: #fff;
  background-color: sandybrown;
  border-radius: 10px;
}
.station_info {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  margin-left: auto;
  margin-right: auto;
}
.station {
  border-width: 1px;
  border-color: #9e9e9e;
  border-style: solid;
  border-radius: 10px;
  text-align: left;
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 10px;
  padding-bottom: 10px;
  box-sizing: border-box;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  margin-top: 20px;
}
</style>

<script>
import axios from "axios"
import Travel from "./components/travel.vue";
import Specia from "./components/specia.vue";
const modal = weex.requireModule('modal')

export default {
  data() {
    return {
      otherTickets: [
        {
          name: "汽车票",
          imgLink: "/web/assets/img/icon_car.png"
        },
        {
          name: "火车票",
          imgLink: "/web/assets/img/icon_train.png"
        },
        {
          name: "飞机票",
          imgLink: "/web/assets/img/jingdian.png"
        }
      ],
      findTabs: ["客运站信息", "国内旅游", "定制客运"],
      stations: [],
      curTabIndex: 0,
      isShow: 'stadion',
      refreshing: false
    };
  },
  created (){
    this.getStations()
  },
  methods: {
    clickTab(index) {
     
      if (this.curTabIndex == index) return;
      this.curTabIndex = index;

      if(index == 0){
        this.isShow = 'stadion'
      }
      if(index == 1){
        this.isShow = 'travel'
      }
      if(index == 2){
        this.isShow = 'specia'
      }

      // modal.toast({
      //     message: this.isShow,
      //     duration: 0.5
      // })
    },
    getStations (){
      const that = this;
      axios.get('http://www.bus365.com/qtrip/app/V1.0/zx_queryhotroute0?businesscode=jdbs%2Cxybs%2Cjcbs&token=%7B%22clienttype%22%3A%22web%22%2C%22ordertoken%22%3A%22undefined%22%2C%22clienttoken%22%3A%22undefined%22%7D')
        .then(function(res){
          res = res.data
          if(res.status == '0000'){
            that.stations = res.hotroutelist
          }
        })
    },
    onrefresh (event) {
        console.log('is refreshing')
        modal.toast({ message: '正在刷新~~~', duration: 1 })
        this.refreshing = true
        setTimeout(() => {
          this.refreshing = false
        }, 2000)
      },
      onpullingdown (event) {
        debugger
        console.log('is onpulling down')
        modal.toast({ message: 'pulling down', duration: 1 })
      }
  },
  components: { Travel,Specia }
};
</script>
