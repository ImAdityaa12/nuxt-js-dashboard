<script setup lang="ts">
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import Chart from "~/components/Chart.vue";
let data = ref([5.2, 5.7, 8.7, 13.9, 18.2, 21.4, 25.0, 22.8, 17.5, 12.1, 7.6]);
let currentCategory = ref("today");
const list = [
  {
    title: "Today",
    component: resolveComponent("TabsToday"),
  },
  {
    title: "Week",
    component: resolveComponent("TabsWeek"),
  },
  {
    title: "Month",
    component: resolveComponent("TabsMonth"),
  },
  {
    title: "Year",
    component: resolveComponent("TabsYear"),
  },
];
function generateRandomValue(number = 7) {
  let values = [];
  for (let j = 0; j < number + 1; j++) {
    values.push(Math.floor(Math.random() * 100));
  }
  data.value = values;
  return values;
}
// const categories = ref<{
//   [key: string]: string[];
// }>({
//   today: [
//     "00:00",
//     "01:00",
//     "02:00",
//     "03:00",
//     "04:00",
//     "05:00",
//     "06:00",
//     "07:00",
//     "08:00",
//     "09:00",
//     "10:00",
//     "11:00",
//     "12:00",
//     "13:00",
//     "14:00",
//     "15:00",
//     "16:00",
//     "17:00",
//     "18:00",
//     "19:00",
//     "20:00",
//     "21:00",
//     "22:00",
//     "23:00",
//   ],
//   week: [
//     "Sunday",
//     "Monday",
//     "Tuesday",
//     "Wednesday",
//     "Thursday",
//     "Friday",
//     "Saturday",
//   ],
//   year: [
//     "Jan",
//     "Feb",
//     "Mar",
//     "Apr",
//     "May",
//     "Jun",
//     "Jul",
//     "Aug",
//     "Sep",
//     "Oct",
//     "Nov",
//     "Dec",
//   ],
// });

// const options = computed(() => ({
//   chart: {
//     type: "line",
//     animation: {
//       enabled: false,
//     },
//   },
//   title: {
//     text: "",
//   },
//   xAxis: {
//     categories:
//       categories.value[currentCategory.value as keyof typeof categories.value],
//     gridLineColor: "transparent",
//     accessibility: {
//       description: "Months of the year",
//     },
//   },
//   yAxis: {
//     gridLineColor: "transparent",
//     title: {
//       text: "",
//     },
//     labels: {
//       format: "{value}",
//     },
//   },
//   tooltip: {
//     crosshairs: true,
//     shared: true,
//   },
//   legend: {
//     enabled: false,
//   },
//   plotOptions: {
//     line: {
//       marker: {
//         enabled: false,
//       },
//       dataLables: {
//         enabled: true,
//       },
//       enableMouseTracking: true,
//     },
//     spline: {
//       marker: {
//         radius: 4,
//         lineColor: "#666666",
//         lineWidth: 1,
//       },
//     },
//   },
//   series: [
//     {
//       name: "line",
//       lineWidth: "4px",
//       color: {
//         linearGradient: { y1: 0, y2: 0, y3: 0, y4: 0 },
//         stops: [
//           [0, "rgba(252,176,69,1)"],
//           [0.33, "rgba(253,29,29,1)"],
//           [0.66, "rgba(131,58,180,1)"],
//           [1, "rgba(29,217,93,1)"],
//         ],
//       },
//       marker: {
//         symbol: "circle",
//       },
//       data: data.value,
//     },
//   ],
// }));
onMounted(() => {
  generateRandomValue(23);
});
const setCategory = (e: any) => {
  currentCategory.value = e.target.innerText.toLowerCase();
  switch (e.target.innerText.toLowerCase()) {
    case "today":
      generateRandomValue(23);
      break;
    case "week":
      generateRandomValue(6);
      break;
    case "month":
      generateRandomValue(11);
      break;
    case "year":
      generateRandomValue(11);
      break;
    default:
      generateRandomValue(23);
  }
};
</script>
<template>
  <div class="grid gap-4">
    <header class="flex items-start justify-between">
      <div class="grow">
        <p>Hi, welcome back User!</p>
        <h1>Dashboard</h1>
      </div>
      <div class="w-[112px] h-[36px] bg-neutral-200"></div>
    </header>
    <main class="grid gap-4">
      <Tabs default-value="Today" @click="setCategory">
        <TabsList class="max-w-[400px]">
          <TabsTrigger
            v-for="item in list"
            :key="item.title"
            :value="item.title"
          >
            {{ item.title }}
          </TabsTrigger>
        </TabsList>
        <TabsContent v-for="item in list" :value="item.title">
          <Chart
            v-if="data.length > 0"
            :current-category="currentCategory"
            :data="data"
          />
        </TabsContent>
      </Tabs>
      <!-- <div>Tabs: Today This week This month This year</div>
      <section>Chart</section> -->
    </main>
    <footer>I'll have 3 different cards</footer>
  </div>
</template>
