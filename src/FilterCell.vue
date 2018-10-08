<template>
  <div>
    <select v-if="cell.options" v-model="cell.filter">
      <option value=""></option>
      <option :key="option" :value="option" v-for="option in cell.options">{{option}}</option>
    </select>
    <input v-else-if="cell.type == 'date'" v-model="cell.filter" type="date">
    <input v-else-if="cell.type == 'number'" v-model="cell.filter" type="number">
    <input v-else v-model="cell.filter" style="width:100%;">
  </div>
</template>

<script>
export default {
  props: ['cell'],
};
</script>

<style>

</style>
