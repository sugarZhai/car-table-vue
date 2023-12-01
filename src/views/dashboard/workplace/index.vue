<template>
  <div class="container">
    <dataView />
    <a-card class="custom-crad">
      <CategoriesPercent />
    </a-card>
    <a-card class="custom-crad">
      <LineItem />
    </a-card>
    <!-- 折线图 -->
    <a-card class="custom-crad">
      <a-row :gutter="10">
        <a-col :span="12"
          ><ChainItem
            :title="$t('City')"
            quota="visitors"
            chart-type="line"
            :card-style="{
              background: 'linear-gradient(180deg, #f2f9fe 0%, #e6f4fe 100%)',
            }"
        /></a-col>
        <a-col :span="12">
          <ChainItem
            :title="$t('Brand')"
            quota="published"
            chart-type="bar"
            :card-style="{
              background: 'linear-gradient(180deg, #F7F7FF 0%, #ECECFF 100%)',
            }"
          />
        </a-col>
      </a-row>
    </a-card>
    <PopularContent />
  </div>
</template>

<script lang="ts" setup>
  import dataView from './components/data-overview.vue';
  import PopularContent from './components/popular-content.vue';
  import CategoriesPercent from './components/categories-percent.vue';
  import ChainItem from './components/chain-item.vue';
  import LineItem from './components/line-item.vue';
</script>

<script lang="ts">
  export default {
    name: 'Dashboard', // If you want the include property of keep-alive to take effect, you must name the component
  };
</script>

<style lang="less" scoped>
  .custom-crad {
    margin-bottom: 15px;
  }
  .container {
    background-color: var(--color-fill-2);
    padding: 16px 20px;
    padding-bottom: 0;
    // display: flex;
  }

  .left-side {
    flex: 1;
    overflow: auto;
  }

  .right-side {
    width: 280px;
    margin-left: 16px;
  }

  .panel {
    background-color: var(--color-bg-2);
    border-radius: 4px;
    overflow: auto;
  }

  :deep(.panel-border) {
    margin-bottom: 0;
    border-bottom: 1px solid rgb(var(--gray-2));
  }

  .moduler-wrap {
    border-radius: 4px;
    background-color: var(--color-bg-2);

    :deep(.text) {
      font-size: 12px;
      text-align: center;
      color: rgb(var(--gray-8));
    }

    :deep(.wrapper) {
      margin-bottom: 8px;
      text-align: center;
      cursor: pointer;

      &:last-child {
        .text {
          margin-bottom: 0;
        }
      }

      &:hover {
        .icon {
          color: rgb(var(--arcoblue-6));
          background-color: #e8f3ff;
        }

        .text {
          color: rgb(var(--arcoblue-6));
        }
      }
    }

    :deep(.icon) {
      display: inline-block;
      width: 32px;
      height: 32px;
      margin-bottom: 4px;
      color: rgb(var(--dark-gray-1));
      line-height: 32px;
      font-size: 16px;
      text-align: center;
      background-color: rgb(var(--gray-1));
      border-radius: 4px;
    }
  }
</style>

<style lang="less" scoped>
  // responsive
  .mobile {
    .container {
      display: block;
    }

    .right-side {
      width: 100%;
      margin-left: 0;
      margin-top: 16px;
    }
  }
</style>
