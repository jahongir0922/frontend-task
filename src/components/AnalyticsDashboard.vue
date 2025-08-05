<template>
  <div class="h-screen bg-white rounded-lg shadow">
    <div class="grid grid-cols-2 gap-4">
      <div class="panel relative">
        <apexchart
          height="325"
          :options="revenueChart"
          :series="revenueSeries"
          class="bg-white dark:bg-black rounded-lg overflow-hidden"
        >
          <!-- loader -->
          <div
            class="min-h-[325px] grid place-content-center bg-white-light/30 dark:bg-dark dark:bg-opacity-[0.08]"
          >
            <span
              class="animate-spin border-2 border-black dark:border-white !border-l-transparent rounded-full w-5 h-5 inline-flex"
            ></span>
          </div>
        </apexchart>
      </div>
      <div class="panel relative">
        <apexchart
          height="325"
          :options="revenueChart"
          :series="revenueSeries"
          class="bg-white dark:bg-black rounded-lg overflow-hidden"
        >
          <!-- loader -->
          <div
            class="min-h-[325px] grid place-content-center bg-white-light/30 dark:bg-dark dark:bg-opacity-[0.08]"
          >
            <span
              class="animate-spin border-2 border-black dark:border-white !border-l-transparent rounded-full w-5 h-5 inline-flex"
            ></span>
          </div>
        </apexchart>
      </div>
      <div class="panel relative">
        <apexchart
          height="325"
          :options="revenueChart"
          :series="revenueSeries"
          class="bg-white dark:bg-black rounded-lg overflow-hidden"
        >
          <!-- loader -->
          <div
            class="min-h-[325px] grid place-content-center bg-white-light/30 dark:bg-dark dark:bg-opacity-[0.08]"
          >
            <span
              class="animate-spin border-2 border-black dark:border-white !border-l-transparent rounded-full w-5 h-5 inline-flex"
            ></span>
          </div>
        </apexchart>
      </div>
      <div class="panel relative">
        <apexchart
          height="325"
          :options="revenueChart"
          :series="revenueSeries"
          class="bg-white dark:bg-black rounded-lg overflow-hidden"
        >
          <!-- loader -->
          <div
            class="min-h-[325px] grid place-content-center bg-white-light/30 dark:bg-dark dark:bg-opacity-[0.08]"
          >
            <span
              class="animate-spin border-2 border-black dark:border-white !border-l-transparent rounded-full w-5 h-5 inline-flex"
            ></span>
          </div>
        </apexchart>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { computed, ref } from "vue";
import apexchart from "vue3-apexcharts";
// interface AnalyticsData {
//   totalUsers: number;
//   totalProducts: number;
//   totalRevenue: number;
//   activeSessions: number;
//   revenueByMonth: Array<{
//     month: string;
//     revenue: number;
//   }>;
// }

// revenue
const revenueChart = computed(() => {
  return {
    chart: {
      height: 325,
      type: "area",
      fontFamily: "Nunito, sans-serif",
      zoom: {
        enabled: false,
      },
      toolbar: {
        show: false,
      },
    },
    dataLabels: {
      enabled: false,
    },
    stroke: {
      show: true,
      curve: "smooth",
      width: 2,
      lineCap: "square",
    },
    dropShadow: {
      enabled: true,
      opacity: 0.2,
      blur: 10,
      left: -7,
      top: 22,
    },
    colors: ["#1b55e2", "#e7515a"],
    markers: {
      discrete: [
        {
          seriesIndex: 0,
          dataPointIndex: 6,
          fillColor: "#1b55e2",
          strokeColor: "transparent",
          size: 7,
        },
        {
          seriesIndex: 1,
          dataPointIndex: 5,
          fillColor: "#e7515a",
          strokeColor: "transparent",
          size: 7,
        },
      ],
    },
    labels: [
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
    xaxis: {
      axisBorder: {
        show: false,
      },
      axisTicks: {
        show: false,
      },
      crosshairs: {
        show: true,
      },
      labels: {
        offsetX: 0,
        offsetY: 5,
        style: {
          fontSize: "12px",
          cssClass: "apexcharts-xaxis-title",
        },
      },
    },
    yaxis: {
      tickAmount: 7,
      labels: {
        formatter: (value: number) => {
          return value / 1000 + "K";
        },
        offsetX: -10,
        offsetY: 0,
        style: {
          fontSize: "12px",
          cssClass: "apexcharts-yaxis-title",
        },
      },
      opposite: false,
    },
    grid: {
      borderColor: "#e0e6ed",
      strokeDashArray: 5,
      xaxis: {
        lines: {
          show: true,
        },
      },
      yaxis: {
        lines: {
          show: false,
        },
      },
      padding: {
        top: 0,
        right: 0,
        bottom: 0,
        left: 0,
      },
    },
    legend: {
      position: "top",
      horizontalAlign: "right",
      fontSize: "16px",
      markers: {
        width: 10,
        height: 10,
        offsetX: -2,
      },
      itemMargin: {
        horizontal: 10,
        vertical: 5,
      },
    },
    tooltip: {
      marker: {
        show: true,
      },
      x: {
        show: false,
      },
    },
    fill: {
      type: "gradient",
      gradient: {
        shadeIntensity: 1,
        inverseColors: !1,
        opacityFrom: 0.28,
        opacityTo: 0.05,
        stops: [45, 100],
      },
    },
  };
});

const revenueSeries = ref([
  {
    name: "Income",
    data: [
      16800, 16800, 15500, 17800, 15500, 17000, 19000, 16000, 15000, 17000,
      14000, 17000,
    ],
  },
]);
</script>
