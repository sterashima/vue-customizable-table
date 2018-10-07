<template>
<vRow :row="header" :useExtra="useExtra">
  <vCell slot-scope="props">
    <!-- @slot Filter cell -->
    <slot :cell="props.cell">
      <select v-if="props.cell.options" v-model="props.cell.filter" style="width:100%;" @change="$emit('filter', filter)">
        <option value=""></option>
        <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
      </select>
      <input type="text" v-model="props.cell.filter" style="width:100%;" @change="$emit('filter', filter)" v-else>
    </slot>
  </vCell>
  
  <vCell cell="" v-if="useExtra" slot="extra" />
</vRow>
</template>

<script lang="ts">
import Vue from 'vue';
import vRow from './Row.vue';
import vCell from './Cell.vue';
interface Head {
  label: string;
  name: string;
  filter: string;
}
export default Vue.extend({
  name: 'vFilterHeader',
  props: {
    /**
     * Extra colunm name
     */
    useExtra: {
      type: Boolean,
      default: false,
    },
    /**
     * Colunm names
     */
    header: {
      type: Array as () => Head[],
      required: true,
    },
  },
  computed: {
    filter(): Map<string, string> {
      return this.header
        .filter( (head: Head): boolean => head.filter !== '')
        .reduce( (filter: Map<string, string>, head: Head): Map<string, string> => {
          filter.set(head.name ? head.name : head.label, head.filter);
          return filter;
        }, new Map());
    },
  },
  components: {
    vCell, vRow,
  },
});
</script>

<style>

</style>

<docs>
### Examples

The simple filter header:

```jsx
<table style="width:100%">
<vFilterHeader :header="[{label: 'one', filter: ''}, {label: 'two', filter: ''}, {label: 'three', filter: ''}, {label: 'four', filter: ''}, {label: 'five', filter: '', options: [1,2,3]} ]"/>
</table>
```

Custom filter header:

```jsx
<table style="width:100%">
<vFilterHeader :header="[{label: 'one', filter: '', type: 'date'}, {label: 'two', filter: '', type: 'number'}, {label: 'three', filter: ''}, {label: 'four', filter: ''}, {label: 'five', filter: '', options: [1,2,3]} ]">
<template slot-scope="props">
  <select v-if="props.cell.options" v-model="props.cell.filter" @change="$emit('filter', filter)">
    <option value=""></option>
    <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
  </select>
  <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter"  @change="$emit('filter', filter)" type="date">
  <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter"  @change="$emit('filter', filter)" type="number">
  <input v-else v-model="props.cell.filter" style="width:100%;" @change="$emit('filter', filter)">
</template>
</vFilterHeader>
</table>
```

</docs>

