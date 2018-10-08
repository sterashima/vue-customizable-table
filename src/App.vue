<template>
  <div id="app">
    <h1>Default table</h1>

    <vTable :rows="rows" :header="header" extraColName="Extra" :page="page" @page="page = $event" :pageLength="10" @filter="filter = $event"/>

    <h1>Custom Pager</h1>
    <vTable :rows="rows" :pageLength="10" :page="page">
      <vPager slot="pager" slot-scope="prop" :value="prop.page" @input="page = $event" :length="prop.length">
        <button slot="prevPage" slot-scope="props" :disabled="props.disabled">Prev</button>
        <button slot-scope="props" :disabled="props.active">{{props.page}}</button>
        <button slot="nextPage" slot-scope="props" :disabled="props.disabled">Next</button>
      </vPager>
    </vTable>

    <h1>Custom filter</h1>
    <span>filter: {{filter}}</span>
    <vTable :rows="rows" :header="header" :pageLength="10" :page="page" @page="page = $event" @filter="filter = $event" :filter="filter">
      <FilterCell slot-scope="props" slot="filter-cell" :cell="props.cell"/>
    </vTable>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import vTable from './components/Table.vue';
import vPager from './components/Pager.vue';
import FilterCell from './FilterCell.vue';

export default Vue.extend({
  name: 'app',
  components: {
    vTable, vPager, FilterCell,
  },
  data() {
    return {
      header: [
        {label: 'one', filter: '', type: 'date'},
        {label: 'two', filter: '', type: 'number'},
        {label: 'three', filter: ''},
        {label: 'four', filter: ''},
        {label: 'five', filter: '', options: [1, 2, 3, 4, 5]},
      ],
      rows: [[1, 2, 3, 4, 5], [1, 2, 3, 4, 5], [1, 2, 3, 4, 5]],
      page: 0,
      filter: {},
    };
  },
});
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
