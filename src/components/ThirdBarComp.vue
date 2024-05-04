<script setup>
import { ref, markRaw, getCurrentInstance, onMounted, onUnmounted } from 'vue';
const mainRef = ref(null);
const secondRef = ref(null);
const thridRef = ref(null);
const chartRef = ref();
const secChartRef = ref();
const thirChartRef = ref();
const { proxy } = getCurrentInstance();
onMounted(() => {
  chartRef.value = markRaw(proxy.$echarts.init(mainRef.value, 'dark', {}));
  chartRef.value.setOption({
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow',
      },
    },
    grid: {
      left: '3%',
      right: '0',
      bottom: '5%',
      containLabel: true,
    },
    tooltip: {
      trigger: 'axis',
    },
    xAxis: [
      {
        name: '寒冷地区',
        nameLocation: 'middle',
        nameGap: 26,
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
        axisTick: { show: false, alignWithLabel: true },
      },
    ],
    yAxis: [
      {
        type: 'value',
      },
    ],
    series: [
      {
        name: 'Direct',
        type: 'bar',
        barWidth: '60%',
        data: [10, 52, 200, 334, 390, 330, 220],
        markLine: {
          symbol: "none",
          label: {
            position: 'insideStartTop',
            fontSize: 14,
            fontWeight: 'bold',
          },
          lineStyle: {
            color: 'red'
          },
          data: [{
            yAxis: 220
          }],
        }
      },
    ],
  });
  secChartRef.value = markRaw(proxy.$echarts.init(secondRef.value, 'dark', {}));
  secChartRef.value.setOption({
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow',
      },
    },
    grid: {
      left: '0',
      right: '0',
      bottom: '5%',
      containLabel: true,
    },
    xAxis: [
      {
        name: '寒冷地区',
        nameLocation: 'middle',
        nameGap: 26,
        type: 'category',
        data: ['', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun', ''],
        axisTick: { show: false, alignWithLabel: true },
      },
    ],
    tooltip: {
      trigger: 'axis',
      formatter(params) {
        return params
          .map(({ seriesName, axisValue, data, value, marker }) => {
            if (data === '-') {
              return '';
            }
            return `
              ${axisValue}<br>
              ${marker} ${seriesName}&ensp;&ensp;&ensp;<strong>${value}</strong>
            `;
          })
          .join('');
      },
    },
    yAxis: [
      {
        axisLabel: {
          show: false,
        },
        type: 'value',
      },
    ],
    series: [
      {
        name: 'Direct',
        type: 'bar',
        barWidth: '60%',
        data: ['-', 10, 52, 100, 234, 320, 460, 120, '-'],
        markLine: {
          symbol: "none",
          lineStyle: {
            color: 'red',
          },
          data: [{
            x: '10%',
            yAxis: 120,
          }],
          label: {
            fontSize: 14,
            fontWeight: 'bold',
            // color: 'red',
            position: 'insideStartTop'
          },
        }
      },
    ],
  });
  thirChartRef.value = markRaw(proxy.$echarts.init(thridRef.value, 'dark', {}));
  thirChartRef.value.setOption({
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow',
      },
    },
    grid: {
      left: '0',
      right: '4%',
      bottom: '5%',
      containLabel: true,
    },
    tooltip: {
      trigger: 'axis',
    },
    xAxis: [
      {
        name: '寒冷地区',
        nameLocation: 'middle',
        nameGap: 26,
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
        axisTick: {
          alignWithLabel: true,
        },
      },
    ],
    yAxis: [
      {
        axisLabel: {
          show: false,
        },
        type: 'value',
      },
    ],
    series: [
      {
        name: 'Direct',
        type: 'bar',
        barWidth: '60%',
        data: [10, 52, 200, 334, 390, 330, 220],
        markLine: {
          symbol: "none",
          lineStyle: {
            color: 'red'
          },
          data: [{
            yAxis: 240
          }],
          label: {
            position: 'insideStartTop',
            fontSize: 14,
            fontWeight: 'bold',
          },
        }
      },
    ],
  });
});

const computedYaxisMax = (chartList) => {
  const maxList = chartList.map(chart => {
    const { _extent } =  chart.getModel().getComponent('yAxis').axis.scale
    const [yMin, yMax] = _extent
    return yMax
  })
  return Math.max(...maxList)
}
const updateYaxisValue = () => {
  let yMin = 0;
  let yMax = computedYaxisMax([chartRef.value, secChartRef.value, thirChartRef.value]);
  console.log('max',yMin, yMax)

  chartRef.value.setOption({
    yAxis: {
      min: yMin,
      max: yMax,
      //interval: interval,
    },
  });
  secChartRef.value.setOption({
    yAxis: {
      min: yMin,
      max: yMax,
      //interval: interval,
    },
  });
  thirChartRef.value.setOption({
    yAxis: {
      min: yMin,
      max: yMax,
      //interval: interval,
    },
  });
};

defineOptions({
  name: 'ThirdBarComp',
});

defineProps({
  msg: String,
});

const count = ref(0);
</script>
<template>
  <h1 @click="updateYaxisValue">{{ msg }}</h1>
  <div class="bar-container">
    <div ref="mainRef" class="bar"></div>
    <div ref="secondRef" class="bar"></div>
    <div ref="thridRef" class="bar"></div>
  </div>
</template>

<style scoped>
.bar-container {
  display: flex;
}

.bar {
  flex: 1;
  height: 80vh;
  min-width: 407px;
}
</style>
