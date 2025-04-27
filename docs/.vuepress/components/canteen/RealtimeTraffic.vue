<template>
  <transition name="fade" mode="out-in" appear>
    <div class="container mt-3">
      <div class="card mb-3" v-for="canteen in trafficList" :key="canteen.canteen_id">
        <div class="card-header">
          <h5 class="card-title">{{ canteen.canteen_name }} {{ canteen.canteen_en_name }}</h5>
          <p class="card-subtitle">平均人数 Avg Number: {{ canteen.avg_number.toFixed(2) }}</p>
          <p class="card-subtitle">更新时间 Last Updated: {{ timeFormat(canteen.time) }}</p>
        </div>
        <ul class="list-group">
          <li class="list-group-item" v-for="booth in canteen.booth_traffic" :key="booth.booth_id">
            <strong>{{ booth.booth_name }} {{ booth.booth_en_name }}</strong> - 排队人数约: {{ booth.avg_number }} 人
          </li>
        </ul>
      </div>
    </div>
  </transition>
</template>

<script>
import { ref } from 'vue';

export default {
  name: "RealtimeTraffic",
  data() {
    return {
      trafficList: [
        {
          canteen_id: 1,
          canteen_name: "荔园食堂",
          canteen_en_name: "Liyuan Canteen",
          avg_number: 15.75,
          time: "2025-04-28T12:59:00",
          booth_traffic: [
            {
              booth_id: 101,
              booth_name: "粤式烧腊",
              booth_en_name: "Cantonese BBQ",
              avg_number: 12
            },
            {
              booth_id: 102,
              booth_name: "麻辣香锅",
              booth_en_name: "Spicy Pot",
              avg_number: 18
            },
            {
              booth_id: 103,
              booth_name: "西式快餐",
              booth_en_name: "Western Fast Food",
              avg_number: 15
            }
          ]
        },
        {
          canteen_id: 2,
          canteen_name: "学生餐厅",
          canteen_en_name: "Student Canteen",
          avg_number: 18.33,
          time: "2025-04-28T12:59:00",
          booth_traffic: [
            {
              booth_id: 201,
              booth_name: "川菜",
              booth_en_name: "Sichuan Cuisine",
              avg_number: 19
            },
            {
              booth_id: 202,
              booth_name: "自助餐",
              booth_en_name: "Buffet",
              avg_number: 16
            },
            {
              booth_id: 203,
              booth_name: "面食窗口",
              booth_en_name: "Noodle Bar",
              avg_number: 20
            }
          ]
        },
        {
          canteen_id: 3,
          canteen_name: "二期餐厅",
          canteen_en_name: "Phase II Canteen",
          avg_number: 13.25,
          time: "2025-04-28T12:59:00",
          booth_traffic: [
            {
              booth_id: 301,
              booth_name: "湘菜",
              booth_en_name: "Hunan Cuisine",
              avg_number: 11
            },
            {
              booth_id: 302,
              booth_name: "水饺",
              booth_en_name: "Dumplings",
              avg_number: 14
            },
            {
              booth_id: 303,
              booth_name: "清真窗口",
              booth_en_name: "Halal Food",
              avg_number: 13
            },
            {
              booth_id: 304,
              booth_name: "小炒",
              booth_en_name: "Stir-fry",
              avg_number: 15
            }
          ]
        }
      ],
      formatter: new Intl.DateTimeFormat('zh-CN', {
        hour12: false,
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit'
      }),
    };
  },

  methods: {
    timeFormat(time) {
      const t = new Date(time);
      return this.formatter.format(t);
    }
  },
};
</script>

<style scoped>
.container {
  font-family: 'Nunito', sans-serif;
  color: #333;
}

.card {
  margin: 10px 0px;
  border: none;
  border-radius: 16px;
  overflow: hidden;
  background-color: #fff;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.card-header {
  color: #333;
  padding: 8px 24px;
  background-color: #fff;
}

.card-subtitle {
  font-size: 0.875rem;
  margin-top: 4px;
}

.list-group {
  list-style-type: none;
  padding-left: 0;
  border-radius: 16px;
}

.list-group-item {
  list-style-type: none;
  font-size: 0.875rem;
  background-color: #f4f4f4;
  padding: 16px 24px;
  border: none;
  transition: background-color 0.3s;
}

.list-group-item:hover {
  background-color: #f8f9fa;
}

/* 淡入淡出效果 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
