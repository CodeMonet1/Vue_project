<template>
  <div>
    <el-button type="info" @click="exit">exit</el-button>
  </div>
</template>

<script>
export default {
  methods: {
    exit() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style>
</style>