<template>
  <a-spin :loading="loading" style="width: 100%">
    <a-card
      class="general-card"
      :header-style="{ paddingBottom: '0' }"
      :body-style="{ padding: '17px 20px 21px 20px' }"
    >
      <template #title>
        {{ $t('workplace.popularContent') }}
      </template>
      <a-space direction="vertical" :size="10" fill>
        <a-form :model="form" @submit="handleSubmit">
          <a-row>
            <a-col :xs="24" :sm="12">
              <a-form-item field="name" label="名称">
                <a-input v-model="form.name" placeholder="请输入名称" />
              </a-form-item>
            </a-col>
            <a-col :xs="24" :sm="12">
              <a-form-item field="city" label="城市">
                <a-select defaultValue="北京" placeholder="Please select ...">
                  <a-option>北京</a-option>
                  <a-option>上海</a-option>
                  <a-option>大连</a-option>
                  <a-option>杭州</a-option>
                </a-select>
              </a-form-item>
            </a-col>
          </a-row>
          <a-row>
            <a-col :xs="24" :sm="12">
              <a-form-item field="month" label="月份">
                <a-month-picker
                  :default-value="`${currentDate.getFullYear()}-${
                    currentMonth < 10 ? '0' : ''
                  }${currentMonth}`"
                />
              </a-form-item>
            </a-col>
            <a-col :xs="24" :sm="12">
              <a-form-item>
                <a-button
                  html-type="submit"
                  type="primary"
                  style="margin-right: 20px"
                  >查询</a-button
                >
                <a-button html-type="submit">重置</a-button>
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
      </a-space>
      <a-space direction="vertical" :size="10" fill>
        <a-table
          :data="renderList"
          row-key="key"
          :pagination="false"
          :bordered="false"
          :scroll="{ x: 'max-content', y: '400px' }"
          :summary="summary"
          :columns="columns"
        >
          <template #summary-cell="{ column, record }">
            <div :style="getColorStyle(column)">{{
              record[column.dataIndex]
            }}</div>
          </template>
        </a-table>
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
  const currentDate = new Date();
  const currentMonth = currentDate.getMonth() + 1; // Note: Months are zero-based
  const form = reactive({
    month: `${currentDate.getFullYear()}-${
      currentMonth < 10 ? '0' : ''
    }${currentMonth}`,
    name: '',
    city: '',
  });
  const handleSubmit = (data: any) => {
    console.log(data);
  };
  const columns: any = computed(() => {
    return [
      {
        title: '序号',
        dataIndex: 'key',
      },
      {
        title: '名称',
        dataIndex: 'orgName',
        // fixed: 'left',
        width: 120,
      },
      {
        title: '编码',
        dataIndex: 'orgNum',
        width: 100,
        ellipsis: true,
        tooltip: true,
      },
      {
        title: '出库占比',
        dataIndex: 'outBoundRatio',
        width: 120,
        sortable: {
          sortDirections: ['ascend', 'descend'],
        },
        render({ record }: { record: any }) {
          const { outBoundRatio } = record;
          const shouldApplyStyles = outBoundRatio <= 1.5;
          const styles = shouldApplyStyles
            ? `color: ${
                outBoundRatio > 1 ? '#ec9536' : '#e83642'
              }; font-weight: bold;`
            : '';

          const tmp = `<span style="${styles}">${outBoundRatio}%</span>`;
          return h(compile(tmp));
        },
      },
      {
        title: '基础商品占比',
        width: 140,
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
    ];
  });
  const getColorStyle: any = (column: { dataIndex: string }) => {
    if (
      ['outBoundRatio', 'grossProfit', 'carMonth', 'outputValue'].includes(
        column.dataIndex
      )
    ) {
      return { color: 'red', fontWeight: 'bolder', textAlign: 'center' };
    }
    return undefined;
  };

  const summary: any = ({ c, data }: { c: any; data: any[] }) => {
    const countData: any = {
      outBoundRatio: 0,
      grossProfit: 0,
      carMonth: 0,
      outputValue: 0,
    };

    data.forEach((re: any) => {
      countData.outBoundRatio += re.outBoundRatio;
      countData.grossProfit += re.grossProfit;
      countData.carMonth += re.carMonth;
      countData.outputValue += re.outputValue;
    });

    // Assuming countData.outBoundRatio is a string
    countData.outBoundRatio = `${parseFloat(countData.outBoundRatio).toFixed(
      2
    )}%`;

    return [
      {
        orgName: '合计',
        ...countData,
      },
    ];
  };

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
          outBoundRatio: 2.71,
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
          outBoundRatio: 0.47,
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
          outBoundRatio: 0.85,
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
          outBoundRatio: 1.43,
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
          outBoundRatio: 0.61,
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
