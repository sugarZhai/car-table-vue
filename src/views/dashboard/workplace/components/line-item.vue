<template>
  <a-card style="margin-bottom: 15px">
    <div class="hor-ver"
      ><div>品牌出库</div>
      <!-- <img
        :src="HorizIcon"
        style="width: 28px"
        @click="updateChartContainerStyle"
      /> -->
    </div>
    <div :style="cardSty"
      ><Chart :style="chartContainerStyle" :option="chartOption"
    /></div>
  </a-card>
</template>

<script lang="ts" setup>
  import { ref, onMounted } from 'vue';
  import useChartOption from '@/hooks/chart-option';
  import useLoading from '@/hooks/loading';
  // import HorizIcon from '@/assets/images/horizon-icon.png';

  const { loading, setLoading } = useLoading(true);
  const xAxis = ref<string[]>([]);
  const textChartsData = ref<number[]>([]);
  const imgChartsData = ref<number[]>([]);
  const videoChartsData = ref<number[]>([]);
  const { chartOption } = useChartOption((isDark) => {
    return {
      grid: {
        left: '7%',
        right: 0,
        top: '20',
        bottom: '60',
      },
      legend: {
        bottom: 0,
        icon: 'circle',
        textStyle: {
          color: '#4E5969',
        },
      },
      xAxis: {
        type: 'category',
        data: xAxis.value,
        axisLine: {
          lineStyle: {
            color: isDark ? '#3f3f3f' : '#A9AEB8',
          },
        },
        axisTick: {
          show: true,
          alignWithLabel: true,
          lineStyle: {
            color: '#86909C',
          },
        },
        axisLabel: {
          color: '#86909C',
        },
      },
      yAxis: {
        type: 'value',
        axisLabel: {
          color: '#86909C',
          formatter(value: number, idx: number) {
            if (idx === 0) return `${value}`;
            return `${value / 1000}k`;
          },
        },
        splitLine: {
          lineStyle: {
            color: isDark ? '#3F3F3F' : '#E5E6EB',
          },
        },
      },
      tooltip: {
        show: true,
        trigger: 'axis',
      },
      series: [
        {
          name: '出库占比',
          data: textChartsData.value,
          stack: 'one',
          type: 'bar',
          barWidth: 10,
          color: isDark ? '#4A7FF7' : '#246EFF',
        },
        {
          name: '出库占比目标',
          data: videoChartsData.value,
          stack: 'one',
          type: 'bar',
          color: isDark ? '#01349F' : '#81E2FF',
          itemStyle: {
            borderRadius: 2,
          },
        },
      ],
    };
  });
  const fetchData = async () => {
    setLoading(true);
    try {
      // const { data: chartData } = await queryContentPublish();
      const generateLineData = (name: string) => {
        const result = {
          name,
          x: [] as string[],
          y: [] as number[],
        };
        new Array(12).fill(0).forEach((_item, index) => {
          result.x.push(`${index + 1}门店`);
          result.y.push(Math.floor(Math.random() * 2001) + 1000);
        });
        return result;
      };
      const chartData = [
        generateLineData('出库占比'),
        generateLineData('出库占比目标'),
      ];
      xAxis.value = chartData[0].x;
      chartData.forEach((el: any) => {
        if (el.name === '出库占比') {
          textChartsData.value = el.y;
        } else if (el.name === '图文类') {
          imgChartsData.value = el.y;
        }
        videoChartsData.value = el.y;
      });
    } catch (err) {
      // you can report use errorHandler or other
    } finally {
      setLoading(false);
    }
  };
  const cardSty: any = ref({
    height: 'auto',
  });
  const chartContainerStyle: any = ref({
    height: '220px',
    transform: 'rotate(0deg)',
  });

  // const updateChartContainerStyle = () => {
  //   if (chartContainerStyle.value.height === '220px') {
  //     chartContainerStyle.value = {
  //       height: '230px',
  //       transform: 'rotate(90deg)', // Rotate 90 degrees
  //     };
  //     cardSty.value = {
  //       height: '481px',
  //       paddingTop: '80px',
  //     };
  //   } else {
  //     chartContainerStyle.value = {
  //       height: '220px',
  //       transform: 'rotate(0deg)',
  //     };
  //     cardSty.value = {
  //       height: 'auto',
  //     };
  //   }
  // };
  onMounted(() => {
    fetchData();
  });
</script>

<style lang="less" scoped>
  .hor-ver {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bolder;
    font-size: 16px;
  }
  /* Add the following media query for landscape mode styles */
  @media screen and (orientation: landscape) {
    .chart-container {
      height: auto; /* or any other specific styles for landscape mode */
    }
  }
</style>
