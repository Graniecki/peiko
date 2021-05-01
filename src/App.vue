<template>
  <div class="container">
    <div class="panel">
      <button class="panel__btn" @click="loadData">Get data</button>
    </div>

    <Warning v-if="catchError" />
    <Loader v-if="loading" />
    <Table :serverData="serverData" />
  </div>
</template>

<script>
import payload from './mocData/index';
import simulateAsyncReq from './plugins/getDataFunc';
import Warning from './components/Warning.vue';
import Loader from './components/Loader.vue';
import Table from './components/Table.vue';

export default {
  data() {
    return {
      serverData: null,
      catchError: false,
      loading: false,
    };
  },
  methods: {
    async loadData() {
      try {
        this.loading = true;
        this.serverData = null;
        this.catchError = false;

        const response = await simulateAsyncReq(payload);
        const data = Object.keys(response).map((key, index) => ({
          id: index + 1,
          stock: response.stocks[index],
          current: response.current[index].toFixed(2),
          change: (response.current[index] - response.start[index]).toFixed(2),
        }));

        this.serverData = data.sort((a, b) => a.stock.localeCompare(b.stock));
        this.loading = false;
      } catch (error) {
        this.loading = false;
        this.catchError = true;
        this.serverData = null;
      }
    },
  },
  components: { Loader, Warning, Table },
};
</script>

<style lang="scss" scoped>
  .container {
    padding-top: 50px;
    width: 500px;
    margin: 0 auto;
    text-align: center;
  }

  .panel {

    &__btn {
      height: 50px;
      width: 200px;
      border: none;
      background-color: #e7e7e7;
      font-size: 18px;
      font-weight: 700;
      border: 1px solid rgb(197, 197, 197);
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 300ms;

      &:hover {
        background-color: #cec8c8;
      }
    }
  }

</style>
