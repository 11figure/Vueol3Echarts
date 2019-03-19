<template>
  <div>
      <div id="map" class="map"></div>
      <div id="chart" class="map"></div>
  </div>
</template>

<script>
      import ADLayer from 'openlayers_echart'
// var oe = new ADLayer(option, map, echarts)
// oe.render()
export default {  
  name: "HelloWorld",
  mounted() {
    this.chart = echarts.init(document.getElementById("chart"));
    this.initData();
  },

  methods: {    
    initData() {
      // //echarts
      // var map = new ol.Map({
      //   layers: [
      //     new ol.layer.Tile({
      //       source: new ol.source.OSM()
      //     })
      //   ],
      //   target: "map",
      //   view: new ol.View({
      //     projection: "EPSG:4326",
      //     center: [103.980167, 30.770697],
      //     zoom: 14
      //   })
      // });

      // // var chart = echarts.init(document.getElementById('chart'));

      var projection = ol.proj.get("EPSG:3857");
    var resolutions = [];
    for(var i=0; i<19; i++){
        resolutions[i] = Math.pow(2, 18-i);
    }
    var tilegrid  = new ol.tilegrid.TileGrid({
        origin: [0,0],
        resolutions: resolutions
    });

    var baidu_source = new ol.source.TileImage({
        projection: projection,
        tileGrid: tilegrid,
        tileUrlFunction: function(tileCoord, pixelRatio, proj){
            if(!tileCoord){
                return "";
            }
            var z = tileCoord[0];
            var x = tileCoord[1];
            var y = tileCoord[2];

            if(x<0){
                x = "M"+(-x);
            }
            if(y<0){
                y = "M"+(-y);
            }

            return "http://online3.map.bdimg.com/onlinelabel/?qt=tile&x="+x+"&y="+y+"&z="+z+"&styles=pl&udt=20151021&scaler=1&p=1";
        }
    });

    var baidu_layer = new ol.layer.Tile({
        source: baidu_source
    });

    var map = new ol.Map({
        target: 'map',
        layers: [baidu_layer],
        view: new ol.View({
            projection: "EPSG:4326",
            center:  [103.980167, 30.770697],
            zoom: 12
        })
    });

      var pt = [103.980167, 30.770697];
      var pie = new ol.Overlay({
        position: pt,
        positioning: "center-center",
        element: document.getElementById('chart')
      });
      map.addOverlay(pie);

      var data = [
        { name: "第一类", value: 20 },
        { name: "第二类", value: 23 },
        { name: "第三类", value: 45 },
        { name: "第四类", value: 34 },
        { name: "第五类", value: 14 }
      ];

      // 指定图表的配置项和数据
      var option = {
        tooltip: {
          trigger: "item",
          formatter: "{b} : {c} ({d}%)"
        },
        toolbox: {
          show: true,
          feature: {
            mark: { show: true },
            magicType: {
              show: true,
              type: ["pie", "funnel"]
            }
          }
        },
        calculable: true,
        series: [
          {
            type: "pie",
            radius: "60%",
            startAngle: "45",
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: "#000000",
                  fontWeight: "bold",
                  fontSize: 16
                }
              }
            },
            lableLine: {
              normal: {
                show: false
              },
              emphasis: {
                show: false
              }
            },
            data: data
          }
        ]
      };
      // 使用刚指定的配置项和数据显示图表。

      var oe = new ADLayer(option,map,echarts) 
      oe.render()
    //   this.chart.setOption(option);
    }
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h1,
    h2 {
    font-weight: normal;
    }
    ul {
    list-style-type: none;
    padding: 0;
    }
    li {
    display: inline-block;
    margin: 0 10px;
    }
    a {
    color: #42b983;
    }
    body, #map {
                border: 0px;
                margin: 0px;
                padding: 0px;
                padding: 0px;
                padding: 0px;
                width: 100%;
                height: 100%;
                font-size: 13px;
            }
            #chart{
                height: 200px;
                width: 200px;
            }
</style>