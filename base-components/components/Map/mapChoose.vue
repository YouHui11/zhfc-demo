<style scoped>
.map{margin: 10px 0} 
.search{margin: 10px ;width: 90%;}
.address{margin: 0;padding: 5px 10px;}
</style>
<template>
    <div class="map">
        <el-input class="search" v-model="addressKeyword" placeholder="请输入地址来直接查找相关位置"></el-input >
        <baidu-map  @ready="mapReady" class="bmView" :scroll-wheel-zoom="true" :center="location" :zoom="zoom" @click="getLocationPoint" :ak="YOUR_APP_KEY">
            <bm-view style="width: 100%; height:300px; "></bm-view>
            <bm-local-search :keyword="addressKeyword" :auto-viewport="true" style="display: none"></bm-local-search>
        </baidu-map>
        <p class="address" v-show="address">已选中：{{address}}</p>
    </div>
</template>
<script>
export default {
  name:'mapAddress',
  data(){
      return{
          YOUR_APP_KEY:'CZodsUXlVUTmM7gI1M0jHgb6',
          addressKeyword:'',
          location:{lng: 114.066112, lat: 22.550918},
          zoom:15,
          geoc:null,
          address:''
      }
  },
  mounted(){
     
  },
  methods:{
    mapReady ({ BMap, map }) {
      const _this = this
      // 获取自动定位方法
      let geolocation = new BMap.Geolocation()
      // 获取逆解析方法实例
      this.geoc = new BMap.Geocoder()
      // 获取自动定位获取的坐标信息
      geolocation.getCurrentPosition(
        function (r) {
          _this.center = {
            lng: r.point.lng,
            lat: r.point.lat
          }
          _this.point = {
            lng: r.point.lng,
            lat: r.point.lat
          }
        },
        { enableHighAccuracy: true }
      )
    },
      getLocationPoint(e){
         this.geoc.getLocation(e.point, (rs) => {
            let addComp = rs.addressComponents;
            this.address = rs.address;
            this.$emit("sureAddress", this.address, rs.point);
         });
      }
  }
}
</script>