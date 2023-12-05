<template>
  <a-card :style="cardSty">
    <div class="hor-ver"
      ><div>出库占比</div>
      <img
        :src="HorizIcon"
        style="width: 28px"
        @click="updateChartContainerStyle"
      />
    </div>
    <Chart :style="chartContainerStyle" :option="chartOption" />
  </a-card>
</template>

<script lang="ts" setup>
  import { ref, onMounted } from 'vue';
  import useChartOption from '@/hooks/chart-option';
  import useLoading from '@/hooks/loading';
  import HorizIcon from '@/assets/images/horizon-icon.png';

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
          barWidth: 16,
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
        new Array(10).fill(0).forEach((_item, index) => {
          result.x.push(`${index + 1}月`);
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
  const cardSty = ref({
    marginBottom: '15px',
    height: 'auto',
    paddingTop: 0,
  });
  const chartContainerStyle = ref({
    height: '220px',
    width: '100%',
    transform: 'rotate(0deg)',
  });

  const updateChartContainerStyle = () => {
    chartContainerStyle.value = {
      height: '230px',
      width: '400px',
      transform: 'rotate(90deg)', // Rotate 90 degrees
    };
    cardSty.value = {
      height: '500px',
      marginBottom: '15px',
      paddingTop: '50px',
    };
  };
  onMounted(() => {
    fetchData();
    // window.addEventListener('orientationchange', updateChartContainerStyle);
    // window.addEventListener('resize', updateChartContainerStyle);
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
