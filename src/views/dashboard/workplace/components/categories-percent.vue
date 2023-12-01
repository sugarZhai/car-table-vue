<template>
  <a-spin :loading="loading" style="width: 100%">
    <Chart height="180px" :option="chartOption" />
  </a-spin>
</template>

<script lang="ts" setup>
  import useLoading from '@/hooks/loading';
  import useChartOption from '@/hooks/chart-option';

  const { loading } = useLoading();
  const { chartOption } = useChartOption((isDark) => {
    const data = [
      // { value: 40, name: '沃尔沃' },
      // { value: 38, name: '日产品牌-英菲' },
      // { value: 22, name: '江苏区奔驰' },
      // { value: 18, name: '一汽奥迪' },
      // { value: 20, name: '雷克萨斯' },
      // { value: 3, name: '捷豹路虎' },
      // { value: 8, name: '南二奔驰' },
      // { value: 10, name: '东北区宝马' },
      // { value: 23, name: '南三奔驰' },
      // { value: 89, name: '西南区宝马' },
      { value: 100, name: '日产品牌' },
      { value: 200, name: '上海区奔驰' },
      { value: 200, name: '西区奔驰' },
      { value: 800, name: '杭州区奔驰' },
      { value: 635, name: '南一奔驰' },
      { value: 580, name: '一丰品牌' },
      { value: 484, name: '本田品牌' },
      { value: 300, name: '广丰品牌' },
      { value: 200, name: '保时捷' },
      { value: 200, name: '一汽奥迪-大众' },
    ];
    return {
      legend: {
        left: 'center',
        data,
        bottom: -100,
        icon: 'circle',
        itemWidth: 8,
        textStyle: {
          color: isDark ? 'rgba(255, 255, 255, 0.7)' : '#4E5969',
        },
        itemStyle: {
          borderWidth: 0,
        },
      },
      tooltip: {
        show: true,
        trigger: 'item',
      },
      series: [
        {
          type: 'pie',
          id: 'distribution',
          radius: ['30%', '80%'],
          label: {
            show: true,
          },
          universalTransition: true,
          animationDurationUpdate: 1000,
          data,
        },
      ],
    };
  });
</script>

<style scoped lang="less"></style>
