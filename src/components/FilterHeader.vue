<template>
<vRow :row="header" :useExtra="useExtra">
  <vCell slot-scope="props">
    <!-- @slot Filter cell -->
    <slot :cell="props.cell">
      <select v-if="props.cell.options" v-model="props.cell.filter">
        <option value=""></option>
        <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
      </select>
      <input type="text" v-model="props.cell.filter" v-else>
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
    /**
     * filter
     */
    value: {
      default: () => ({}),
    },
  },
  watch: {
    header: {
      handler: function(v) {
        const filter = v.filter( (head: Head): boolean => head.filter !== '')
        .reduce( (f: any, head: Head): any => {
          f[(head.name ? head.name : head.label)] = head.filter;
          return f;
        }, {});
        this.$emit('input', filter);
      },
      deep: true,
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

```vue
<template>
<div>
  <table style="width:100%">
    <vFilterHeader :header="header" v-model="filter"/>
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

Custom filter header:

```vue
<template>
<div>
  <table style="width:100%">
    <vFilterHeader :header="header" v-model="filter">
      <template slot-scope="props">
        <select v-if="props.cell.options" v-model="props.cell.filter">
          <option value=""></option>
          <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
        </select>
        <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter" type="date">
        <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter" type="number">
        <input v-else v-model="props.cell.filter">
      </template>
    </vFilterHeader>
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

