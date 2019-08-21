<template>
  <div>
    <input v-model.number="a">
    <br>
    <input v-model.number="b">
    <h2>The sum is {{ c }}</h2>
  </div>
</template>

<script>
export default {
  data: () => ({
    a: 2,
    b: 3
  }),
  computed: {
    c() {
      return this.a + this.b
    }
  },
  watch: {
    c(val) {

    }
  }
}
</script>

<style scoped>
input {
  color: black !important;
}
</style>
