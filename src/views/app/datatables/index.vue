<template>
  <div className="container mt-4">
    <JqueryTable/>
  </div>
</template>
<script>
import JqueryTable from '../../../components/JqueryTable/JqueryTable.vue'

export default {
  name: 'App',
  components: {
    JqueryTable
  }
}
</script>
<style>
.container {
  max-width: 1000px;
}
</style>
