<template>
  <div>
    <h1>Records</h1>
    <button @click="getRecords">Refresh</button>
    <div v-for="(r, index) of records" :key="index">
      {{ index + 1 }} - {{ r.elapsedTime }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      records: [],
    };
  },
  created() {
    console.log("Records Created");
    this.getRecords();
  },
  methods: {
    getRecords() {
      console.log("Run getRecords methods");
      const records = JSON.parse(localStorage.getItem("records")) || [];
      this.records = records;
    },
  },
};
</script>

<style scoped>
</style>