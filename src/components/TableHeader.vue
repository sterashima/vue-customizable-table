<template>
  <thead>
    <vSimpleHeader :labels="labels" :extraColName="extraColName" :sort="sort"/>
    <vFilterHeader :header="header" :useExtra="extraColName != null" :value="value" @input="$emit('input', $event)" v-if="$scopedSlots.filter || useFilterHeader">
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
interface Sort {
  label: string;
  isAsc: boolean;
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
    /**
     * header definition
     */
    header: {
      type: Array as () => Head[],
    },
    /**
     * name of extra col
     */
    extraColName: {
      type: String,
      default: null,
    },
    /**
     * useFilterHeader
     */
    useFilterHeader: {
      type: Boolean,
      default: false,
    },
    /**
     * filter values
     */
    value: {
      default: () => ({}),
    },
    /**
     * sort
     */
    sort: {
      type: Object as () => Sort,
      default() {
        return {label: '', isAcs: true};
      },
    },
  },
});
</script>

<style scoped>
tr:last-child th,
tr:last-child td {
  border-bottom: 1px black solid;
}
</style>


<docs>
### Examples

The simple table header:

```vue
<template>
<div>
  <table style="width:100%">
    <vTableHeader :header="header" :useFilterHeader="true" v-model="filter"/>
  </table>
  <div>filter: {{filter}}</div>
</div>
</template>

<script>
export default {
  data() {
    return {
      filter: {},
      header: [
        {label: 'one', filter: ''}, 
        {label: 'two', filter: ''}, 
        {label: 'three', filter: ''}, 
        {label: 'four', filter: ''}, 
        {label: 'five', filter: '', options: [1,2,3]}
      ]
    };
  },
}
</script>
```

The table header without filter:

```jsx
<table style="width:100%">
<vTableHeader :header="['one','two','three','four','five' ]"/>
</table>
```

Custom table header:

```vue
<template>
<div>
  <table style="width:100%">
    <vTableHeader :header="header" :useFilterHeader="true" v-model="filter">
      <template slot-scope="props" slot="filter">
        <select v-if="props.cell.options" v-model="props.cell.filter">
          <option value=""></option>
          <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
        </select>
        <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter" type="date">
        <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter" type="number">
        <input v-else v-model="props.cell.filter" style="width:100%;">
      </template>
    </vTableHeader>
  </table>
  <div>filter: {{filter}}</div>
</div>
</template>

<script>
export default {
  data() {
    return {
      filter: {},
      header: [
        {name: 'date', label: 'one', filter: '', type: 'date'}, 
        {name: 'number',label: 'two', filter: '', type: 'number'}, 
        {label: 'three', filter: ''}, 
        {label: 'four', filter: ''}, 
        {label: 'five', filter: '', options: [1,2,3]}
      ]
    };
  },
}
</script>
```

</docs>

