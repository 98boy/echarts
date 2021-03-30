<template>
  <div class="wrapper">
    <div class="map-container" ref="myEchart"></div>
    <div id="small" ref="smallEchart"></div>
    <div class="xz" ref="xz"></div>
    <div id="div" @mousewheel="zoom">
      <img id="img" src="../assets/dt.jpg" alt="" @mousedown="mousedown" @mousemove="move"/>
    </div>
  </div>
</template>

<script>
import echarts from "echarts";
// import 'echarts/map/js/china'
import geoJson from './china.json'
import xz from './xizang.json'
// import jwp from './mapShZb'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      option1: {
        tooltip: {
          trigger: 'item',
          formatter: '{b}:{c}'
        },
        visualMap: {
          inRange: {
            color: ['rgb(5,107,108)', 'rgb(22,197,187)']
          },
          x: 'left',
          y: 'bottom',
          text: ['多', '少'],
          calculable: false,
          orient: "horizontal",
          seriesIndex: [0],
          show: false
        },
        geo: {
          map: null, // 地图类型
          show: true, // 是否显示地理坐标系组件
          // 是否开启鼠标缩放和平移漫游 默认不开启 如果只想要开启缩放或者平移，
          // 可以设置成 'scale' 或者 'move' 设置成 true 为都开启
          roam: false,
          // 图形上的文本标签
          label: {
            show: true // 是否显示对应地名
          },
          // 地图区域的多边形 图形样式
          itemStyle: {
            normal: {
              areaColor: '#031525',
              borderColor: '#3B5077',
            },
            emphasis: {
              areaColor: '#2B91B7',
            }
          },
          // // 高亮状态下的多边形和标签样式
          // emphasis: {
          //   label: {
          //     show: true, // 是否显示标签
          //     color: '#fff' // 文字的颜色 如果设置为 'auto'，则为视觉映射得到的颜色，如系列色
          //   },
          //   itemStyle: {
          //     areaColor: '#aeaeae' // 地图区域的颜色
          //   }
          // }
        },
        series: [{
          type: 'map',
          map: null,
          roam: false,
          itemStyle: {
            normal: { label: { show: true } },
            emphasis: { label: { show: true } }
          }
        }]
      },
      geoCoordMap: {
        '拉萨': [91.1865, 30.1465],
        '呼和浩特': [111.670801, 40.818311],
        '昆明': [102.9199, 25.4663],
        '成都': [103.9526, 30.7617]
      },
      xjData: [
        [{
          name: '拉萨'
        }, {
          name: '拉萨',
          vale: 300
        }],
        [{
          name: '拉萨'
        }, {
          name: '呼和浩特',
          vale: 120
        }],
        [{
          name: '拉萨'
        }, {
          name: '昆明',
          vale: 59
        }],
        [{
          name: '拉萨'
        }, {
          name: '成都',
          vale: 32
        }]
      ],
      linesData: [
        {
          naem: '呼和浩特',
          coords: [[91.1865, 30.1465], [111.670801, 40.818311]]
        },
        {
          naem: '昆明',
          coords: [[91.1865, 30.1465], [102.9199, 25.4663]]
        },
        {
          naem: '成都',
          coords: [[91.1865, 30.1465], [103.9526, 30.7617]]
        },
        {
          naem: '拉萨',
          coords: [[91.1865, 30.1465], [80.9526, 16]]
        },
      ],
      planePath: 'path://M1705.06,1318.313v-89.254l-319.9-221.799l0.073-208.063c0.521-84.662-26.629-121.796-63.961-121.491c-37.332-0.305-64.482,36.829-63.961,121.491l0.073,208.063l-319.9,221.799v89.254l330.343-157.288l12.238,241.308l-134.449,92.931l0.531,42.034l175.125-42.917l175.125,42.917l0.531-42.034l-134.449-92.931l12.238-241.308L1705.06,1318.313z',
      zoomVal: 1,
      zoomX:null,
      zoomY:null
    }
  },
  methods: {
    map() {
      var _this = this
      let myEchart = echarts.init(this.$refs.myEchart);
      echarts.registerMap('china', geoJson);
      let option = {
        title: {
          text: '产业分布图',
          x: 'left'
        },
        visualMap: {
          inRange: {
            color: ['rgb(5,107,108)', 'rgb(22,197,187)']
          },
          x: 'left',
          y: 'bottom',
          text: ['多', '少'],
          calculable: false,
          orient: "horizontal",
          seriesIndex: [0]
        },
        tooltip: {
          trigger: 'item',
          formatter: '{b}:{c}'
        },
        geo: {
          map: 'china', // 地图类型
          show: true, // 是否显示地理坐标系组件
          // 是否开启鼠标缩放和平移漫游 默认不开启 如果只想要开启缩放或者平移，
          // 可以设置成 'scale' 或者 'move' 设置成 true 为都开启
          roam: false,
          // 图形上的文本标签
          label: {
            show: true // 是否显示对应地名
          },
          // 地图区域的多边形 图形样式
          itemStyle: {
            normal: {
              areaColor: '#031525',
              borderColor: '#3B5077',
            },
            emphasis: {
              areaColor: '#2B91B7',
            }
          },
          // // 高亮状态下的多边形和标签样式
          // emphasis: {
          //   label: {
          //     show: true, // 是否显示标签
          //     color: '#fff' // 文字的颜色 如果设置为 'auto'，则为视觉映射得到的颜色，如系列色
          //   },
          //   itemStyle: {
          //     areaColor: '#aeaeae' // 地图区域的颜色
          //   }
          // }
        },
        series: [
          {
            type: 'map',
            map: 'china',
            label: {
              normal: {
                show: true
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: '#fff'
                }
              }
            },
            roam: false,
            itemStyle: {
              normal: {
                areaColor: 'rgb(22,197,187)',
                borderColor: '#0227ad',
              },
              emphasis: {
                areaColor: '#2B91B7'
              }
            },
            data: [
              { name: '北京', value: 200 },
              { name: '天津', value: 100 },
              { name: '上海', value: 110 },
              { name: '重庆', value: 30 },
              { name: '河北', value: 30 },
              { name: '河南', value: 32 },
              { name: '云南', value: 640 },
              { name: '辽宁', value: 20 },
              { name: '黑龙江', value: 11 },
              { name: '湖南', value: 0 },
              { name: '安徽', value: 39 },
              { name: '山东', value: 130 },
              { name: '新疆', value: 103 },
              { name: '江苏', value: 135 },
              { name: '浙江', value: 106 },
              { name: '江西', value: 132 },
              { name: '湖北', value: 115 },
              { name: '广西', value: 195 },
              { name: '甘肃', value: 400 },
              { name: '山西', value: 460 },
              { name: '内蒙古', value: 100 },
              { name: '陕西', value: 650 },
              { name: '吉林', value: 632 },
              { name: '福建', value: 258 },
              { name: '贵州', value: 369 },
              { name: '广东', value: 456 },
              { name: '青海', value: 321 },
              { name: '西藏', value: 132 },
              { name: '四川', value: 164 },
              { name: '宁夏', value: 290 },
              { name: '海南', value: 370 },
              { name: '台湾', value: 463 },
              { name: '香港', value: 432 },
              { name: '澳门', value: 509 }
            ]
          },
          {
            type: 'scatter',
            coordinateSystem: 'geo',
            itemStyle: {
              normal: {
                color: 'blue'
              }
            },
            lable: {
              format: '{c}'
            },
            data: [
              {
                name: '内蒙古',
                value: [111.670801, 40.818311, 340]
              },
              {
                name: '新疆',
                value: [87.617733, 43.792818, 600]
              },
              {
                name: '青海',
                value: [101.778916, 36.623178, 18]
              },
              {
                name: '江西',
                value: [115.892151, 28.676493, 460]
              },
              {
                name: '拉萨',
                value: [91.1865, 30.1465, 300]
              },
              {
                name: '呼和浩特',
                value: [111.670801, 40.818311, 120]
              },
              {
                name: '昆明',
                value: [102.9199, 25.4663, 59]
              },
              {
                name: '成都',
                value: [103.9526, 30.7617, 32]
              }
            ],
            zlevel: 2
          },
          {
            type: 'scatter',
            symbol: 'pin',
            symbolSize: 50,
            coordinateSystem: 'geo',
            itemStyle: {
              normal: {
                color: '#F62157', //标志颜色
              }
            },
            data: [
              {
                name: '内蒙古',
                value: [111.670801, 40.818311, 340]
              },
              {
                name: '新疆',
                value: [87.617733, 43.792818, 600]
              },
              {
                name: '青海',
                value: [101.778916, 36.623178, 18]
              },
              {
                name: '江西',
                value: [115.892151, 28.676493, 460]
              }
            ],
            zlevel: 2
          },
          {
            name: '连线',
            type: 'lines',
            zlevel: 3,
            effect: {
              show: true,
              period: 6,
              trailLength: 0.2,
              symbol: _this.planePath,
              // symbol: 'rect',
              symbolSize: 15
            },
            lineStyle: {
              normal: {
                color: '#3ed4ff',
                width: 1,
                opacity: 0.4,
                curveness: 0.2
              }
            },
            // data: this.convertData(_this.xjData)
            data: _this.linesData
          }
        ]
      };
      myEchart.setOption(option);
      myEchart.on('click', function (params) {
        let smallEchart = echarts.init(document.getElementById('small'))
        switch (params.name) {
          case '内蒙古': {
            let NM = require('./neimeng.json')
            echarts.registerMap(`${params.name}`, NM);
            _this.option1.series[0].map = params.name
            _this.option1.geo.map = params.name
            _this.option1.series[0].data = [
              { name: '呼和浩特市', value: 20 },
              { name: '包头市', value: 0 },
              { name: '乌海市', value: 30 },
              { name: '赤峰市', value: 0 },
              { name: '通辽市', value: 10 },
              { name: '鄂尔多斯市', value: 0 },
              { name: '呼伦贝尔市', value: 10 },
              { name: '巴彦淖尔市', value: 0 },
              { name: '乌兰察布市', value: 20 },
              { name: '兴安盟', value: 0 },
              { name: '锡林郭勒盟', value: 10 },
              { name: '阿拉善盟', value: 0 }
            ]
            let a = {
              type: 'scatter',
              coordinateSystem: 'geo',
              itemStyle: {
                color: 'red'
              },
              data: [
                {
                  name: '呼和浩特市',
                  value: [111.670801, 40.818311, 20]
                },
                {
                  name: '乌海市',
                  value: [106.825563, 39.673734, 30]
                }
              ]
            }
            _this.option1.series.push(a)
            smallEchart.setOption(_this.option1)
            break;
          }
          case '新疆': {
            let XJ = require('./xinjiang.json')
            echarts.registerMap(`${params.name}`, XJ);
            _this.option1.series[0].map = params.name
            _this.option1.geo.map = params.name
            smallEchart.setOption(_this.option1)
            break
          }
          case '山西': {
            let SX = require('./shanxi.json')
            echarts.registerMap(`${params.name}`, SX);
            _this.option1.series[0].map = params.name
            _this.option1.geo.map = params.name
            _this.option1.series[0].data = [
              { name: '大同市', value: 120 },
              { name: '朔州市', value: 0 },
              { name: '忻州市', value: 340 },
              { name: '吕梁市', value: 0 },
              { name: '太原市', value: 0 },
              { name: '阳泉市', value: 0 },
              { name: '晋中市', value: 0 },
              { name: '临汾市', value: 0 },
              { name: '长治市', value: 0 },
              { name: '晋城市', value: 0 },
              { name: '运城市', value: 0 },
            ]
            let a = {
              type: 'scatter',
              coordinateSystem: 'geo',
              itemStyle: {
                color: 'red'
              },
              data: [
                {
                  name: '大同市',
                  value: [113.295259, 40.09031, 120]
                },
                {
                  name: '忻州市',
                  value: [112.733538, 38.41769, 340]
                }
              ]
            }
            _this.option1.series.push(a)
            smallEchart.setOption(_this.option1)
            break
          }
        }
      })
    },
    convertData(data) {
      var res = [];
      for (var i = 0; i < data.length; i++) {
        var dataItem = data[i];
        var fromCoord = this.geoCoordMap[dataItem[0].name];
        var toCoord = this.geoCoordMap[dataItem[1].name];
        if (fromCoord && toCoord) {
          res.push([{
            coord: fromCoord
          }, {
            coord: toCoord
          }]);
        }
      }
      return res;
    },
    // 左下角的西藏地图
    xz() {
      var _this = this
      let myEchart = echarts.init(this.$refs.xz);
      echarts.registerMap('xz', xz);
      let option = {
        visualMap: {
          inRange: {
            color: ['rgb(5,107,108)', 'rgb(22,197,187)']
          },
          x: 'left',
          y: 'bottom',
          text: ['多', '少'],
          calculable: false,
          orient: "horizontal",
          seriesIndex: [0],
          show: false
        },
        geo: {
          map: 'xz',
          show: true,
          roam: false,
          itemStyle: {
            normal: {
              areaColor: '#031525',
              borderColor: '#3B5077',
            },
            emphasis: {
              areaColor: '#2B91B7',
            }
          },
        },
        series: [
          {
            map: 'xz',
            type: 'map',
            tooltip: {
              trigger: 'item'
            },
            data: [
              {
                name: '拉萨市',
                value: 30
              },
              {
                name: '阿里地区',
                value: 62
              },
              {
                name: '那曲地区',
                value: 59
              },
              {
                name: '日喀则市',
                value: 0
              },
              {
                name: '山南市',
                value: 130
              },
              {
                name: '林芝市',
                value: 90
              },
              {
                name: '昌都市',
                value: 60
              }
            ]
          },
          {
            type: 'scatter',
            coordinateSystem: 'geo',
            itemStyle: {
              color: 'yellow'
            },
            data: [
              {
                name: '拉萨市',
                value: [91.132212, 29.660361, 30]
              }
            ]
          }
        ]
      }
      myEchart.setOption(option);
    },
    // 图片跟随滚轮的放大缩小
    zoom(event) {
      console.log('###############', event.wheelDelta)
      var o = document.getElementsByTagName("img")[0]
      var params = {
        zoomVal: 3,
        left: 0,
        top: 0,
        currentX: 0,
        currentY: 0,
        flag: false
      }
      if (event.wheelDelta > 0) {

        this.zoomVal++
      } else {
        // o.style.transform = "scale(" + this.zoomVal + ")";
        this.zoomVal--
        if (this.zoomVal <= 0) {
          this.zoomVal = 1
        }
      }
      o.style.transformOrigin=[this.zoomX,this.zoomY]
      o.style.transform = "scale(" + this.zoomVal + ")";
      // if (params.zoomVal >= 0.2) {
      //   o.style.transform = "scale(" + params.zoomVal + ")";
      // } else {
      //   params.zoomVal = 0.2;
      //   o.style.transform = "scale(" + params.zoomVal + ")";
      //   return false;
      // }
    },
   
    mousedown(e) {
      var img = document.getElementById('img')
      var div=document.getElementById('div')
      //按下的时候获取元素的初始位置和鼠标的初始位置
      var eleX = img.offsetLeft;
		  var eleY = img.offsetTop;
		  var startX = e.clientX;
		  var startY = e.clientY;
      img.setCapture&&img.setCapture();
      document.onmousemove=function(e){
        //可以获取鼠标的结束位置
				var endX = e.clientX;
				var endY = e.clientY;
        //求出鼠标的距离差
				var disX = endX - startX;
				var disY = endY - startY;
        //求出元素移动的最终位置  =  元素的初始位置  + 鼠标的距离差
				var lastX = eleX + disX;
				var lastY = eleY + disY;
        //把求出来的最终位置设置给元素
				img.style.left = lastX + 'px';
				img.style.top = lastY + 'px';
        document.onmouseup = function(){
          console.log(document.onmousemove)
					document.onmousemove = document.onmouseup = null;
					img.releaseCapture&&img.releaseCapture();//低版本浏览器释放全局捕获
				}
      }
    },
    move(e){
      this.zoomX=e.offsetLeft+'px'
      this.zoomY=e.offsetTop+'px'
      // console.log(e)
    }
  },
  mounted() {
    this.map()
    this.xz()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  width: 1600px;
  height: 700px;
  position: relative;
  display: flex;
  background: #ccc;
}
.map-container {
  position: absolute;
  left: 0;
  width: 700px;
  height: 700px;
  z-index: 2;
}
#small {
  position: absolute;
  right: 600px;
  bottom: 100px;
  width: 300px;
  height: 300px;
  border: 1px solid #000;
}
.xz {
  position: absolute;
  width: 230px;
  height: 200px;
  border: 1px solid #000;
  left: 0;
  bottom: 30px;
  z-index: 0;
}
#div {
  position: absolute;
  width: 300px;
  height: 300px;
  right: 0;
  overflow: hidden;
  border: 1px solid #000;
}
#div img {
  position: absolute;
  width: 150px;
  height: 150px;
  /* margin-left: 50%; */
  /* transform: translateX(-50%) translateY(-50%); */
  /* margin-top: 50%; */
}
</style>
