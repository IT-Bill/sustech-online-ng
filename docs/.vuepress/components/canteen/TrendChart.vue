<template>
  <div id="bustable">
    <object-selector :objs="{
      '中心餐厅 | Center Cafeteria': true,
      '十栋餐厅 | Building 10 No.2 Cafeteria': false
    }" v-slot="canteenProps">
      <br />
      <br />
      <object-selector :objs="canteenProps.selected ? {
        '麻辣烫 | Spicy Hot Pot': 11,
        '大众菜左 | Popular Dishes': 12,
        '大众菜右 | Popular Dishes': 13,
        '风味面食 | Noodles': 14,
        '潮汕卤味套餐 | Chiu Chow-style Brino Meat': 15,
        '铁锅拌饭 | Rice with Mixed Vegetables': 16
      } : { '大众菜左 | Popular Dishes': 21, '大众菜右 | Popular Dishes': 22 }
        " v-slot="boothProps">
        <div v-for="meal in [0, 1, 2]" :key="meal">
          <v-chart class="echarts" :option="getChartData(boothProps.selected, meal)" />
        </div>
      </object-selector>
    </object-selector>
  </div>
</template>

<script>
import ECharts from 'vue-echarts';
import { use } from 'echarts/core';

import { CanvasRenderer } from 'echarts/renderers';
import { LineChart } from 'echarts/charts';
import { GridComponent, TooltipComponent, TitleComponent, LegendComponent, DataZoomComponent } from 'echarts/components';

import ObjectSelector from "../bus/ObjectSelector.vue";

use([
  CanvasRenderer,
  LineChart,
  GridComponent,
  TooltipComponent,
  TitleComponent,
  LegendComponent,
  DataZoomComponent
]);

export default {
  name: "TrendChart",
  components: {
    'object-selector': ObjectSelector,
    'v-chart': ECharts
  },

  data() {
    return {
      mockData: {
        // 中心餐厅 - Center Cafeteria
        '11': { // 麻辣烫
          0: this.generateBreakfastData(5, 1.5), // 早餐
          1: this.generateLunchData(8, 1.2),    // 午餐
        },
        '12': { // 大众菜左
          0: this.generateBreakfastData(7, 1.1),
          1: this.generateLunchData(10, 0.9),
        },
        '13': { // 大众菜右
          0: this.generateBreakfastData(6, 0.9),
          1: this.generateLunchData(12, 1.3),
        },
        '14': { // 风味面食
          0: this.generateBreakfastData(10, 1.4),
          1: this.generateLunchData(6, 0.8),
        },
        '15': { // 潮汕卤味套餐
          0: this.generateBreakfastData(2, 0.7),
          1: this.generateLunchData(9, 1.1),
        },
        '16': { // 铁锅拌饭
          0: this.generateBreakfastData(4, 1.2),
          1: this.generateLunchData(11, 1.0),
        },
        
        // 十栋餐厅 - Building 10
        '21': { // 大众菜左
          0: this.generateBreakfastData(3, 0.8),
          1: this.generateLunchData(7, 1.2),
        },
        '22': { // 大众菜右
          0: this.generateBreakfastData(5, 1.0),
          1: this.generateLunchData(9, 1.4),
        }
      }
    }
  },

  methods: {
    // 生成早餐数据 (7:00-9:00，高峰在7:50和8:50)
    generateBreakfastData(baseValue, factor) {
      const result = [];
      // 生成7:00到9:00的数据，每分钟一个数据点
      for (let hour = 7; hour < 9; hour++) {
        for (let minute = 0; minute < 60; minute++) {
          const time = `${hour.toString().padStart(2, '0')}:${minute.toString().padStart(2, '0')}`;
          
          // 创建两个高峰：7:50左右和8:50左右
          let peakFactor = 1;
          
          // 第一个高峰：7:50前后
          if (hour === 7 && minute >= 40 && minute <= 55) {
            peakFactor = 1 + ((1 - Math.abs(minute - 50) / 10) * factor);
          }
          // 第二个高峰：8:50前后
          else if (hour === 8 && minute >= 40 && minute <= 55) {
            peakFactor = 1 + ((1 - Math.abs(minute - 50) / 10) * factor);
          }
          
          // 添加一些随机波动
          const randomFactor = 0.9 + Math.random() * 0.2;
          const number = Math.round(baseValue * peakFactor * randomFactor);
          
          result.push({
            time: time,
            number: number
          });
        }
      }
      return result;
    },
    
    // 生成午餐数据 (11:00-13:00，高峰在12:00左右)
    generateLunchData(baseValue, factor) {
      const result = [];
      // 生成11:00到13:00的数据，每分钟一个数据点
      for (let hour = 11; hour < 13; hour++) {
        for (let minute = 0; minute < 60; minute++) {
          const time = `${hour.toString().padStart(2, '0')}:${minute.toString().padStart(2, '0')}`;
          
          // 创建高峰：12:00左右
          let peakFactor = 1;
          
          // 距离12:00越近，人数越多
          const minutesFrom12 = Math.abs((hour - 12) * 60 + minute);
          if (minutesFrom12 < 30) {
            peakFactor = 1 + ((1 - minutesFrom12 / 30) * factor);
          }
          
          // 添加一些随机波动
          const randomFactor = 0.9 + Math.random() * 0.2;
          const number = Math.round(baseValue * peakFactor * randomFactor);
          
          result.push({
            time: time,
            number: number
          });
        }
      }
      return result;
    },
    
    // 生成晚餐数据 (17:00-19:00，高峰在18:00左右) - 添加这个是为了完整性
    generateDinnerData(baseValue, factor) {
      const result = [];
      // 生成17:00到19:00的数据，每分钟一个数据点
      for (let hour = 17; hour < 19; hour++) {
        for (let minute = 0; minute < 60; minute++) {
          const time = `${hour.toString().padStart(2, '0')}:${minute.toString().padStart(2, '0')}`;
          
          // 创建高峰：18:00左右
          let peakFactor = 1;
          
          // 距离18:00越近，人数越多
          const minutesFrom18 = Math.abs((hour - 18) * 60 + minute);
          if (minutesFrom18 < 30) {
            peakFactor = 1 + ((1 - minutesFrom18 / 30) * factor);
          }
          
          // 添加一些随机波动
          const randomFactor = 0.9 + Math.random() * 0.2;
          const number = Math.round(baseValue * peakFactor * randomFactor);
          
          result.push({
            time: time,
            number: number
          });
        }
      }
      return result;
    },

    getChartData(boothId, meal) {
      const trafficList = this.mockData[boothId]?.[meal] || [];
      const titles = ["早餐 Breakfast", "午餐 Lunch", "晚餐 Dinner"];
      
      return {
        title: {
          text: titles[meal],
        },
        xAxis: {
          type: 'category',
          data: trafficList.map(item => item.time),
        },
        yAxis: {
          type: 'value',
          name: '人数 Number',
        },
        series: [{
          data: trafficList.map(item => item.number),
          type: 'line',
          lineStyle: {
            color: "rgb(90,120,200)"
          }
        }],
        dataZoom: [{
          type: 'slider',
          xAxisIndex: 0,
          start: 0,
          end: 100
        }]
      };
    }
  }
}
</script>

<style>
.echarts {
  width: 100%;
  height: 250px;
}
</style>
