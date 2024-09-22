<script setup lang="ts">
import { ref, computed, onMounted, watch } from "vue";
import { Chart } from "highcharts";
const props = defineProps<{
  currentCategory: string;
  data: number[];
}>();

const categories = ref<{
  [key: string]: string[];
}>({
  today: [
    "00:00",
    "01:00",
    "02:00",
    "03:00",
    "04:00",
    "05:00",
    "06:00",
    "07:00",
    "08:00",
    "09:00",
    "10:00",
    "11:00",
    "12:00",
    "13:00",
    "14:00",
    "15:00",
    "16:00",
    "17:00",
    "18:00",
    "19:00",
    "20:00",
    "21:00",
    "22:00",
    "23:00",
  ],
  week: [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
  ],
  year: [
    "Jan",
    "Feb",
    "Mar",
    "Apr",
    "May",
    "Jun",
    "Jul",
    "Aug",
    "Sep",
    "Oct",
    "Nov",
    "Dec",
  ],
});

const chartData = ref(props.data);
const options = computed(() => ({
  chart: {
    type: "line",
    animation: { enabled: false },
  },
  title: { text: "" },
  xAxis: {
    categories: categories.value[props.currentCategory],
    gridLineColor: "transparent",
    accessibility: { description: "Time periods" },
  },
  yAxis: {
    gridLineColor: "transparent",
    title: { text: "" },
    labels: { format: "{value}" },
  },
  tooltip: {
    crosshairs: true,
    shared: true,
  },
  legend: { enabled: false },
  plotOptions: {
    line: {
      marker: { enabled: false },
      dataLabels: { enabled: false },
      enableMouseTracking: true,
    },
    spline: {
      marker: {
        enabled: false,
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
      marker: { symbol: "circle" },
      data: chartData.value,
    },
  ],
}));

function generateMonth() {
  const currentDate = new Date();
  const currentMonth = currentDate.getMonth() + 1;
  const currentYear = currentDate.getFullYear();

  const daysInMonth = new Date(currentYear, currentMonth, 0).getDate();
  const monthDates = Array.from({ length: daysInMonth }, (_, i) => {
    const day = String(i + 1).padStart(2, "0");
    const month = String(currentMonth).padStart(2, "0");
    return `${month}/${day}`;
  });

  categories.value = { ...categories.value, month: monthDates };
}

onMounted(() => {
  generateMonth();
});

watch(
  () => props.data,
  (newData) => {
    chartData.value = newData;
  },
  { deep: true }
);
const chartInstance = ref<Chart | null>(null);

watch(
  () => props.currentCategory,
  () => {
    nextTick(() => {
      if (chartInstance.value) {
        chartInstance.value.update({
          xAxis: {
            categories: categories.value[props.currentCategory],
          },
        });
      }
    });
  }
);
</script>

<template>
  <div class="border p-4 rounded-lg">
    <highchart
      v-if="chartData.length > 0"
      :options="options"
      @chart-init="chartInstance = $event"
    />
  </div>
</template>
