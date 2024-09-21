<script setup lang="ts">
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
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
let categories = ref({
  today: [],
  week: [],
  month: [],
  year: [],
});
let currentCategory = ref([]);
const options = computed(() => ({
  chart: {
    type: "line",
    animation: {
      enabled: false,
    },
  },
  title: {
    text: "",
  },
  xAxis: {
    categories: currentCategory,
    gridLineColor: "transparent",
    accessibility: {
      description: "Months of the year",
    },
  },
  yAxis: {
    gridLineColor: "transparent",
    title: {
      text: "",
    },
    labels: {
      format: "{value}°",
    },
  },
  tooltip: {
    crosshairs: true,
    shared: true,
  },
  legend: {
    enabled: false,
  },
  plotOptions: {
    line: {
      marker: {
        enabled: false,
      },
      dataLables: {
        enabled: true,
      },
      enableMouseTracking: true,
    },
    spline: {
      marker: {
        radius: 4,
        lineColor: "#666666",
        lineWidth: 1,
      },
    },
  },
  series: [
    {
      name: "line",
      lineWidth: "4px",
      color: {
        linearGradient: { y1: 0, y2: 0, y3: 0, y4: 0 },
        stops: [
          [0, "rgba(252,176,69,1)"],
          [0.33, "rgba(253,29,29,1)"],
          [0.66, "rgba(131,58,180,1)"],
          [1, "rgba(29,217,93,1)"],
        ],
      },
      marker: {
        symbol: "square",
      },
      data: [5.2, 5.7, 8.7, 13.9, 18.2, 21.4, 25.0, 22.8, 17.5, 12.1, 7.6],
    },
  ],
}));
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
      <Tabs default-value="Today" class="w-[400px]">
        <TabsList>
          <TabsTrigger
            v-for="item in list"
            :key="item.title"
            :value="item.title"
          >
            {{ item.title }}
          </TabsTrigger>
        </TabsList>
        <TabsContent v-for="item in list" :value="item.title">
          <highchart :options="options" />
        </TabsContent>
      </Tabs>
      <!-- <div>Tabs: Today This week This month This year</div>
      <section>Chart</section> -->
    </main>
    <footer>I'll have 3 different cards</footer>
  </div>
</template>
