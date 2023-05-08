<template>
  <div class="map-wrapper">
      <div id="map" ></div>
      <div class="right-board">
        <div class="flex column">
          <q-btn label="translate" @click="translate"></q-btn>
          <div class="flex right-board__title">{{ $t('츠루미구') }}</div>
          <div class="house-card" v-if="currentBuilding">
            <div class="house-card__left"></div>
            <div class="house-card__right">
              <div class="house-card__title">
                <span>{{ currentBuilding.labelText }} </span>
                <span class="sub">
                  ( {{ currentBuilding.adress }} )
                </span>
              </div>
              <div class="house-card__content" v-for="(room,index) in currentBuilding.rooms" :key="index">
                <div class="house-card__image">
                  <img :src="getImgUrl(room.img)" alt="" >
                </div>
                <div class="house-card__floor">
                {{ room.floor  }}{{ $t('층') }}
                </div>
                <div class="house-card__price">
                  <div class="price">{{ room.price }}{{ $t('만엔') }}</div>
                  <div class="maintain-price">{{ room.maintainPrice }}{{ $t('엔') }}</div>
                </div>
                <div class="house-card__fee">
                  <div class="fee1"> {{$t('시키킹')}} {{ room.fee1 }}{{ room.fee1 == '-' ? '' : $t('만엔') }}</div>
                  <div class="fee2"> {{ $t('레이킹') }} {{ room.fee2 }}{{ room.fee2 == '-' ? '' : $t('만엔') }}</div>
                </div>
                <div class="house-card__area">
                  <div class="area1">{{ room.area1 }}</div>
                  <div class="area2">{{ room.area2 }}</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import ComputedMixin from "../ComputedMixin";
import UtilMethodMixin from "../UtilMethodMixin";

export default {
  mixins: [ComputedMixin, UtilMethodMixin],
  data(){
    return {
      drawerRight : true,
      currentBuilding:null,
      roomDatas:null,
      markers:[],
      map:null,
    }
  },
  methods:{
    translate(){

      console.log()
      if(this.$i18n.locale == 'ja'){
        this.$i18n.locale =''
      }else{
        this.$i18n.locale = 'ja'
      }
      console.log(this.$i18n.locale)
      this.updateRooms()
      this.updateMarkers()
    },
    updateMarkers(){
      const thisObj = this;
      thisObj.markers.map(maker=>{
        maker.setMap();
      })
      this.roomDatas.map(room=>{
        console.log(room)
        const marker = new google.maps.Marker({
          id: room.id,
          label: { text: room.rooms.length + thisObj.$t('건'), color: "white", className: 'marker-position', },
          icon: thisObj.getImgUrl("apartment-with-bubble.png"),
          position: room.position,
          map: thisObj.map,
        });
        thisObj.markers.push(marker)

        const infoWindow = new google.maps.InfoWindow({
          content: "",
          disableAutoPan: true,
        });
        const markerClickFunc = (currentMaker) => {
          console.log(currentMaker)
          thisObj.currentBuilding = {
            labelText: thisObj.roomDatas.filter(i => i.id == currentMaker.id)[0].labelText,
            label: thisObj.roomDatas.filter(i => i.id == currentMaker.id)[0].label,
            adress: thisObj.roomDatas.filter(i => i.id == currentMaker.id)[0].adress,
            id: currentMaker.id,
            rooms: thisObj.roomDatas.filter(i => i.id == currentMaker.id)[0].rooms,
          }
          infoWindow.setContent(thisObj.currentBuilding.labelText);

          infoWindow.open(thisObj.map, currentMaker);
        }
        marker.addListener("click", () => {
          markerClickFunc(marker)
        });
      })
    },
    getGeoCode(address){
      const thisObj = this;
      return new Promise((resolve,reject)=>{
        
        var geocoder = new google.maps.Geocoder();

        geocoder.geocode({ 'address': address }, function (results, status) {

          if (status == google.maps.GeocoderStatus.OK) {
            var latitude = results[0].geometry.location.lat();
            var longitude = results[0].geometry.location.lng();
            resolve(thisObj.returnPosition(`${latitude}, ${longitude}`));
          }
        });
      })
    },
    returnPosition (string) {
      let data = string.replaceAll(" ", "").split(",");
      console.log(data)
      return { lat: parseFloat(data[0]), lng: parseFloat(data[1]) };
    },
    async updateRooms(){
      const thisObj =this;
      // const adress1 = "34.707374, 135.5741839";// インプレイス鶴見緑地
      // const adress2 = "34.7062751, 135.5721787"; // アクエルド緑地公園
      // const adress3 = "34.7002751, 135.5721787"; // アクエルド緑地公園
      const adress1 = "大阪市鶴見区諸口1丁目";// インプレイス鶴見緑地
      const adress2 = "大阪市鶴見区諸口2丁目"; // アクエルド緑地公園
      const adress3 = "大阪市鶴見区諸口3丁目"; // アクエルド緑地公園
      const adress4 = "大阪市鶴見区諸口4丁目"; // アクエルド緑地公園
      const adress5 = "大阪市鶴見区諸口5丁目"; // アクエルド緑地公園
      const adress6 = "大阪市鶴見区諸口6丁目"; // アクエルド緑地公園
      const position1  = await thisObj.getGeoCode(adress1)
      const position2  = await thisObj.getGeoCode(adress2)
      const position3  = await thisObj.getGeoCode(adress3)
      const position4  = await thisObj.getGeoCode(adress4)
      const position5  = await thisObj.getGeoCode(adress5)
      const position6  = await thisObj.getGeoCode(adress6)
      thisObj.roomDatas = [
        {
          id: 1,
          labelText: thisObj.$t('인플레이스츠루미녹지'),
          img: "building-2.png",
          position: position1,
          adress: thisObj.$t(adress1),
          rooms: [
            {
              img: "image-1.png",
              floor: 2,
              fee1: "-",
              fee2: 6,
              area1: "1DK",
              area2: "34.2m2",
              price: 6,
              maintainPrice: "7000"
            },
            {
              img: "image-1.png",
              floor: 3,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "34.2m2",
              price: 6.2,
              maintainPrice: "7000"
            },
            {
              img: "image-1.png",
              floor: 5,
              fee1: "-",
              fee2: 6.3,
              area1: "1DK",
              area2: "34.2m2",
              price: 6.3,
              maintainPrice: "7000"
            },
          ],
        },
        {
          id: 2,
          img: "building-1.png",
          labelText: thisObj.$t('아쿠에르도 녹지공원'),
          position: position2,
          adress: thisObj.$t('오사카부 오사카시 쓰루미구 미도리 5'),
          rooms: [
            {
              img: "image-3.png",
              floor: 3,
              fee1: "-",
              fee2: "-",
              area1: "1DK",
              area2: "27.28m2",
              price: 6.4,
              maintainPrice: "9400"
            },
            {
              img: "image-2.png",
              floor: 7,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "43.04m2",
              price: 8,
              maintainPrice: "10400"
            },
          ]
        },
        {
          id: 3,
          img: "building-1.png",
          labelText: thisObj.$t('아쿠에르도 녹지공원'),
          position: position3,
          adress: thisObj.$t('오사카부 오사카시 쓰루미구 모로구치 1쵸메'),
          rooms: [
            {
              img: "image-3.png",
              floor: 3,
              fee1: "-",
              fee2: "-",
              area1: "1DK",
              area2: "27.28m2",
              price: 6.4,
              maintainPrice: "9400"
            },
            {
              img: "image-2.png",
              floor: 7,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "43.04m2",
              price: 8,
              maintainPrice: "10400"
            },
          ]
        },
        {
          id: 4,
          img: "building-1.png",
          labelText: thisObj.$t('아쿠에르도 녹지공원'),
          position: position4,
          adress: thisObj.$t('오사카부 오사카시 쓰루미구 모로구치 1쵸메'),
          rooms: [
            {
              img: "image-3.png",
              floor: 3,
              fee1: "-",
              fee2: "-",
              area1: "1DK",
              area2: "27.28m2",
              price: 6.4,
              maintainPrice: "9400"
            },
            {
              img: "image-2.png",
              floor: 7,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "43.04m2",
              price: 8,
              maintainPrice: "10400"
            },
          ]
        },
        {
          id: 5,
          img: "building-1.png",
          labelText: thisObj.$t('아쿠에르도 녹지공원'),
          position: position5,
          adress: thisObj.$t('오사카부 오사카시 쓰루미구 모로구치 1쵸메'),
          rooms: [
            {
              img: "image-3.png",
              floor: 3,
              fee1: "-",
              fee2: "-",
              area1: "1DK",
              area2: "27.28m2",
              price: 6.4,
              maintainPrice: "9400"
            },
            {
              img: "image-2.png",
              floor: 7,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "43.04m2",
              price: 8,
              maintainPrice: "10400"
            },
          ]
        },
        {
          id: 6,
          img: "building-1.png",
          labelText: thisObj.$t('아쿠에르도 녹지공원'),
          position: position6,
          adress: thisObj.$t('오사카부 오사카시 쓰루미구 모로구치 1쵸메'),
          rooms: [
            {
              img: "image-3.png",
              floor: 3,
              fee1: "-",
              fee2: "-",
              area1: "1DK",
              area2: "27.28m2",
              price: 6.4,
              maintainPrice: "9400"
            },
            {
              img: "image-2.png",
              floor: 7,
              fee1: "-",
              fee2: 6.2,
              area1: "1DK",
              area2: "43.04m2",
              price: 8,
              maintainPrice: "10400"
            },
          ]
        },
      ];

      if(thisObj.currentBuilding){
        thisObj.currentBuilding = thisObj.roomDatas.filter(i=>i.id == thisObj.currentBuilding.id)[0];
      }
    }
  },
  mounted () {
    // this.$i18n.locale = 'ja';
    // this.$i18n.locale = '';
    const thisObj = this;

    async function initMap () {


      const yokozutumi = { lat: 34.703961420378484, lng: 135.5707830665288 };
      thisObj.map = new google.maps.Map(document.getElementById("map"), {
        center: yokozutumi,
        zoom: 16,
        mapTypeId: google.maps.MapTypeId.ROADMAP,
        styles: 
        {
          featureType: "poi",
          elementType: "labels",
          stylers: [
            { visibility: "off" }
          ]
        }
      });

      await thisObj.updateRooms()
      thisObj.updateMarkers()
      

      console.log(thisObj.markers)
      new markerClusterer.MarkerClusterer({ markers: thisObj.markers, map: thisObj.map });
    }

    

    window.initMap = initMap();
  },
};
</script>

<style>
.page-name {}
</style>
<style lang="scss">
#map {
  width: calc(100% - 600px);
  height: 100%;
}
#q-app,
html,
body ,
.map-wrapper{
  height: 100%;
  margin: 0;
  padding: 0;
}
.map-wrapper{
  display: flex;
}

.right-board{
  width: 600px;
  padding: 30px;
  &__title{
    font-size: 24px;
    font-weight: bold;
  }
}

.marker-position{
    margin-bottom:33px;
}
.house-card{
  &__content{
    display: flex;
    padding:  5px 0;
    border-bottom: 1px solid #ddd;
  }
  &__image{
    display: flex;
    img{
      width: 100px;
    }
  }
  &__floor{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 1;
    font-size: 16px;
  }
  &__price{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-left: 5px;
    flex: 1;
    font-size: 16px;
    .price{
      color:#f82055;
      font-size: 20px;
      font-weight: bold;
    }
  }
  &__fee{
    display: flex;
    flex-direction: column;
    font-size: 16px;
    justify-content: center;
    align-items: flex-start;
    padding-left: 5px;
    flex: 1;
  }
  &__area{
    flex: 1;
    display: flex;
    flex-direction: column;
    font-size: 16px;
    justify-content: center;
    align-items: center;
    padding-left: 5px;
  }
}
</style>
