<template>
  <div>
      <div id="map" class="map"></div>
      <div id="chart" class="map"></div>
  </div>
</template>

<script>
import ADLayer from "openlayers_echart";
// var oe = new ADLayer(option, map, echarts)
// oe.render()
export default {
  name: "HeatMap",
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
      for (var i = 0; i < 19; i++) {
        resolutions[i] = Math.pow(2, 18 - i);
      }
      var tilegrid = new ol.tilegrid.TileGrid({
        origin: [0, 0],
        resolutions: resolutions
      });

      var baidu_source = new ol.source.TileImage({
        projection: projection,
        tileGrid: tilegrid,
        tileUrlFunction: function(tileCoord, pixelRatio, proj) {
          if (!tileCoord) {
            return "";
          }
          var z = tileCoord[0];
          var x = tileCoord[1];
          var y = tileCoord[2];

          if (x < 0) {
            x = "M" + -x;
          }
          if (y < 0) {
            y = "M" + -y;
          }

          return (
            "http://online3.map.bdimg.com/onlinelabel/?qt=tile&x=" +
            x +
            "&y=" +
            y +
            "&z=" +
            z +
            "&styles=pl&udt=20151021&scaler=1&p=1"
          );
        }
      });

      var baidu_layer = new ol.layer.Tile({
        source: baidu_source
      });

      var map = new ol.Map({
        target: "map",
        layers: [baidu_layer],
        view: new ol.View({
          projection: "EPSG:4326",
          center: [103.980167, 30.770697],
          zoom: 12
        })
      });

      var pt = [103.980167, 30.770697];
      var pie = new ol.Overlay({
        position: pt,
        positioning: "center-center",
        element: document.getElementById("chart")
      });
      map.addOverlay(pie);

      //热力图 图例
      var heatData = [];
      for (var i = 0; i < 20; ++i) {
        heatData.push([
          400 + Math.random() * 300,
          5 + Math.random() * 10,
          Math.random()
        ]);
      }
      for (var i = 0; i < 100; ++i) {
        heatData.push([
          100 + Math.random() * 600,
          150 + Math.random() * 50,
          Math.random()
        ]);
      }
      for (var i = 0; i < 200; ++i) {
        heatData.push([
          Math.random() * 1000,
          Math.random() * 800,
          Math.random() * 0.5
        ]);
      }
      var option = {
        title: {
          text: "热力图自定义样式"
        },
        series: [
          {
            type: "heatmap",
            data: heatData,
            hoverable: false,
            gradientColors: [
              {
                offset: 0.4,
                color: "green"
              },
              {
                offset: 0.5,
                color: "yellow"
              },
              {
                offset: 0.8,
                color: "orange"
              },
              {
                offset: 1,
                color: "red"
              }
            ],
            minAlpha: 0.2,
            valueScale: 2,
            opacity: 0.6
          }
        ]
      };

      // 使用刚指定的配置项和数据显示图表。

      // var oe = new ADLayer(option, map, echarts);
      // oe.render();
        this.chart.setOption(option);
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
body,
#map {
  border: 0px;
  margin: 0px;
  padding: 0px;
  padding: 0px;
  padding: 0px;
  width: 100%;
  height: 100%;
  font-size: 13px;
}
#chart {
  height: 200px;
  width: 200px;
}
</style>