<template>
  <a-spin :loading="loading" style="width: 100%">
    <a-card
      class="general-card"
      :header-style="{ paddingBottom: '0' }"
      :body-style="{ padding: '17px 20px 21px 20px' }"
    >
      <template #title>
        {{ '汽车用品统计表' }}
      </template>
      <a-space direction="vertical" :size="10" fill>
        <a-table
          :columns="cloneColumns"
          :data="dataList"
          :summary="cloneSummary"
          :pagination="false"
          :scroll="{ x: 'max-content', y: '400px' }"
        />
      </a-space>
    </a-card>
  </a-spin>
</template>

<script lang="ts" setup>
  import { computed, ref, h, compile, reactive } from 'vue';
  import useLoading from '@/hooks/loading';
  // import { queryPopularList } from '@/api/dashboard';

  const { loading, setLoading } = useLoading();
  const renderList = ref<any>();
  const dataList = ref<any>();
  const cloneColumns = ref<any>();
  const cloneSummary: any = ({ c, data }) => {
    const countD = {
      grossProfit: 0,
      outBoundRatio: 0,
      basicCommodityRatio: 0,
    };
    data.forEach(
      (record: {
        grossProfit: number;
        outBoundRatio: number;
        basicCommodityRatio: number;
      }) => {
        countD.grossProfit += record.grossProfit;
        countD.outBoundRatio += record.outBoundRatio;
        countD.basicCommodityRatio += record.basicCommodityRatio;
      }
    );

    return [
      {
        orgName: '合计',
        grossProfit: countD.grossProfit,
        outBoundRatio: countD.outBoundRatio,
        basicCommodityRatio: countD.basicCommodityRatio,
      },
    ];
  };
  cloneColumns.value = reactive([
    {
      title: '序号',
      dataIndex: 'key',
    },
    {
      title: '名称',
      dataIndex: 'orgName',
      fixed: 'left',
      width: 140,
    },
    {
      title: '编码',
      dataIndex: 'orgNum',
      width: 100,
      ellipsis: true,
      tooltip: true,
    },
    {
      title: '毛利',
      dataIndex: 'grossProfit',
      // eslint-disable-next-line consistent-return
      summaryCellStyle: (record: { grossProfit: number }) => {
        if (record.grossProfit > 10) {
          return {
            backgroundColor: 'rgb(var(--arcoblue-6))',
            color: '#fff',
          };
        }
      },
    },
    {
      title: '出库占比',
      dataIndex: 'outBoundRatio',
      width: 120,
      sortable: {
        sortDirections: ['ascend', 'descend'],
      },
    },
    {
      title: '基础商品占比',
      dataIndex: 'basicCommodityRatio',
      sortable: {
        sortDirections: ['ascend', 'descend'],
      },
    },
    {
      title: '新车销量(台)',
      width: 120,
      dataIndex: 'carMonth',
    },
    {
      title: '单车占比',
      width: 90,
      dataIndex: 'unitOutputRatio',
    },
    {
      title: '产值(万元)',
      width: 100,
      dataIndex: 'outputValue',
      // eslint-disable-next-line consistent-return
      summaryCellStyle: (record: { outputValue: number }) => {
        if (record.outputValue > 1) {
          return {
            color: 'red',
            textAlign: 'center',
          };
        }
      },
    },
    {
      title: '单车产值(万元)',
      width: 140,
      dataIndex: 'unitOutputValue',
    },
    {
      title: '成本(万元)',
      width: 100,
      dataIndex: 'cost',
    },
    {
      title: '毛利(万元)',
      width: 100,
      dataIndex: 'grossProfit',
    },
    {
      title: '固定单产(元)',
      width: 120,
      dataIndex: 'determineUnitOutput',
    },
  ]);
  dataList.value = reactive([
    {
      key: '1',
      orgName: 'Jane Doe',
      grossProfit: 23000,
      outBoundRatio: 10,
      basicCommodityRatio: 8,
      orgNum: 'Expand Content',
    },
    {
      key: '2',
      orgName: 'Alisa Ross',
      grossProfit: 25000,
      outBoundRatio: 9,
      basicCommodityRatio: -12,
    },
    {
      key: '3',
      orgName: 'Kevin Sandra',
      grossProfit: 22000,
      outBoundRatio: 15,
      basicCommodityRatio: -2,
      orgNum: '1',
    },
    {
      key: '4',
      orgName: 'Ed Hellen',
      grossProfit: 17000,
      outBoundRatio: 2,
      basicCommodityRatio: 3,
      orgNum: '2',
    },
    {
      key: '5',
      orgName: 'William Smith',
      grossProfit: 27000,
      outBoundRatio: 11,
      basicCommodityRatio: 0,
      orgNum: '3',
    },
  ]);
  const fetchData = async () => {
    try {
      setLoading(true);
      // const { data } = await queryPopularList({ type: contentType });
      renderList.value = [
        {
          key: 1,
          id: 1,
          basicCommodityRatio: '1.28',
          carMonth: 20,
          cost: '30',
          grossProfit: 1,
          grossProfitRatio: '1.2',
          orgName: '沃尔沃',
          orgNum: 'woerwo',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 7,
          unitOutputRatio: '2',
          unitOutputValue: '10',
          determineUnitOutput: 12,
        },
        {
          key: 2,
          id: 2,
          basicCommodityRatio: '1.28',
          carMonth: 21,
          cost: '30',
          grossProfit: 2,
          grossProfitRatio: '1.2',
          orgName: '日产-英菲',
          orgNum: 'yingfei',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 9,
          unitOutputRatio: '2',
          unitOutputValue: '10',
          determineUnitOutput: 13,
        },
        {
          key: 3,
          id: 3,
          basicCommodityRatio: '1.28',
          carMonth: 22,
          cost: '30',
          grossProfit: 4,
          grossProfitRatio: '1.2',
          orgName: '江苏奔驰',
          orgNum: 'jiangsubenchi',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 11,
          unitOutputRatio: '2',
          unitOutputValue: '10',
          determineUnitOutput: 14,
        },
        {
          key: 4,
          id: 4,
          basicCommodityRatio: '1.28',
          carMonth: 23,
          cost: '30',
          grossProfit: 3,
          grossProfitRatio: '1.2',
          orgName: '一汽奥迪',
          orgNum: 'yiqiaodi',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 30,
          unitOutputRatio: '2',
          unitOutputValue: '10',
          determineUnitOutput: 15,
        },
        {
          key: 5,
          id: 5,
          basicCommodityRatio: '1.28',
          carMonth: 24,
          cost: '30',
          grossProfit: 2,
          grossProfitRatio: '1.2',
          orgName: '雷克萨斯',
          orgNum: 'leikesasi',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 22,
          unitOutputRatio: '2',
          unitOutputValue: '10',
          determineUnitOutput: 16,
        },
        {
          key: 6,
          id: 6,
          basicCommodityRatio: '1.28',
          carMonth: 25,
          cost: '30',
          grossProfit: 2,
          grossProfitRatio: '1.2',
          orgName: '捷豹路虎',
          orgNum: 'jiebaoluhu',
          outBoundGoalAchievementRate: '0.2',
          outBoundRatio: '2.73',
          outBoundRatioGoal: '2.8',
          outputValue: 21,
          unitOutputRatio: '2',
          unitOutputValue: '10',
        },
      ];
    } catch (err) {
      // you can report use errorHandler or other
    } finally {
      setLoading(false);
    }
  };
  fetchData();
</script>

<style scoped lang="less">
  .general-card {
    min-height: 395px;
  }
  :deep(.arco-table-tr) {
    height: 44px;
    .arco-typography {
      margin-bottom: 0;
    }
  }
  .increases-cell {
    display: flex;
    align-items: center;
    span {
      margin-right: 4px;
    }
  }
</style>
