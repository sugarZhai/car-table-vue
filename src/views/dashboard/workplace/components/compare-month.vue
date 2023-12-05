<template>
  <!-- 折线图 -->
  <a-card class="custom-crad">
    <a-row :gutter="10">
      <a-col :span="12"
        ><ChainItem
          :title="'产值'"
          quota="visitors"
          chart-type="line"
          :card-style="{
            background: 'linear-gradient(180deg, #f2f9fe 0%, #e6f4fe 100%)',
          }"
      /></a-col>
      <a-col :span="12">
        <ChainItem
          :title="'出库'"
          quota="published"
          chart-type="bar"
          :card-style="{
            background: 'linear-gradient(180deg, #F7F7FF 0%, #ECECFF 100%)',
          }"
        />
      </a-col>
    </a-row>
  </a-card>
</template>

<script lang="ts" setup>
  import ChainItem from './chain-item.vue';
</script>

<style scoped lang="less"></style>
