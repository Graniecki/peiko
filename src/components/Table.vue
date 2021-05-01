<template>
  <div class="table" v-if="serverData">
    <div class="table__header">
      <div class="table__item">Stock</div>
      <div class="table__item">Current</div>
      <div class="table__item">Change</div>
    </div>

    <div class="table__body">
      <div class="table__row" v-for="item in serverData" :key="item.key">
        <div class="table__data">{{ item.stock }}</div>
        <div class="table__data">{{ item.current }}</div>
        <div
          class="table__data"
          :class="item.change < 0 ? 'loss' : 'profit'"
        >
          {{ item.change >= 0 ? '+' : '' }}{{ item.change }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['serverData'],
};
</script>

<style lang="scss" scoped>
  @mixin centered {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .table {
    margin-top: 50px;
    border: 1px solid rgb(197, 197, 197);
    border-radius: 5px;

    &__header,
    &__row {
      height: 50px;
      display: flex;
    }

    &__header {
      background-color: #e7e7e7;
    }

    &__row {
      background-color: rgb(253, 253, 255);
      transition: background-color 300ms;

      &:hover {
        background-color: rgb(235, 243, 252);
      }
    }

    &__data{
      @include centered;
      flex: 1 0 33%;
      border-top: 1px solid rgb(197, 197, 197);
      font-size: 20px;
      font-weight: 600;
      cursor: default;
    }

    &__item {
      @include centered;
      flex: 1 0 33%;
      font-size: 25px;
      font-weight: 700;
    }
  }

  .profit {
    color: rgb(106, 241, 106);
  }

  .loss {
    color: rgb(235, 51, 51);
  }
</style>
