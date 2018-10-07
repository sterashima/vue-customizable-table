<template>
  <thead>
    <vSimpleHeader :labels="labels" :extraColName="extraColName"/>
    <vFilterHeader :header="header" :useExtra="extraColName != null" @filter="$emit('filter', $event)">
      <template slot-scope="props" v-if="$scopedSlots.filter">
        <!-- @slot filter cell -->
        <slot name="filter" :cell="props.cell"></slot>
      </template>
    </vFilterHeader>
  </thead>
</template>

<script lang="ts">
import Vue from 'vue';
import vSimpleHeader from './SimpleHeader.vue';
import vFilterHeader from './FilterHeader.vue';
interface Head {
  label: string;
}
export default Vue.extend({
  name: 'vTableHeader',
  components: {
    vSimpleHeader, vFilterHeader,
  },
  computed: {
    labels(): string[] {
      if (!this.header) {
        return [];
      }
      return this.header.map((header: (string|Head)): string => (typeof header === 'string') ? header : header.label );
    },
  },
  props: {
    header: {
      type: Array as () => Head[],
    },
    extraColName: {
      type: String,
      default: null,
    },
  },
});
</script>

<style scoped>
thead tr:last-child th,
thead tr:last-child td {
  border-bottom: 1px black solid;
}
</style>


<docs>
### Examples

The simple table header:

```jsx
<table style="width:100%">
<vTableHeader :header="[{label: 'one', filter: ''}, {label: 'two', filter: ''}, {label: 'three', filter: ''}, {label: 'four', filter: ''}, {label: 'five', filter: '', options: [1,2,3,4]} ]"/>
</table>
```


Custom table header:

```jsx
<table style="width:100%">
<vTableHeader :header="[{label: 'one', filter: '', type: 'date'}, {label: 'two', filter: '', type: 'number'}, {label: 'three', filter: ''}, {label: 'four', filter: ''}, {label: 'five', filter: '', options: [1,2,3]} ]">
<template slot-scope="props" slot="filter">
  <select v-if="props.cell.options" v-model="props.cell.filter" @change="$emit('filter', filter)">
    <option value=""></option>
    <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
  </select>
  <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter"  @change="$emit('filter', filter)" type="date">
  <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter"  @change="$emit('filter', filter)" type="number">
  <input v-else v-model="props.cell.filter" style="width:100%;" @change="$emit('filter', filter)">
</template>
</vTableHeader>
</table>
```
</docs>

