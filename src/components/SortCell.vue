<template>
<vCell @click="$emit('sort', label)">
  <span>{{label}}</span>
  <span v-if="isSort">
    <slot v-if="isAsc" name="asc">
      <span>&darr;</span>
    </slot>
    <slot v-else name="desc">
      <span>&uarr;</span>
    </slot>
  </span>
  <span v-else>&#8597;</span>
</vCell>
</template>

<script lang="ts">
/**
 * The table cell
 */
import vCell from './HeaderCell.vue';
import Vue from 'vue';
export default Vue.extend({
  name: 'vSortCell',
  components: {
    vCell,
  },
  props: {
    /**
     * Value of Cell
     */
    label: {
      required: true,
    },
    /**
     * sort this col ?
     */
    isSort: {
      type: Boolean,
      default: false,
    },
    /**
     * sort direction is ASC ?
     */
    isAsc: {
      type: Boolean,
      default: true,
    },
   },
});
</script>

<style scoped>
th {
  cursor: pointer;
}
</style>

<docs>
### Examples

The simple cell:

```jsx
<table style="width:100%">
<tr>
<vCell cell="Cell Value"/>
</tr>
</table>
```

Custom content:

```jsx
<table style="width:100%">
<tr>
<vCell :cell="{text: 'red', color: 'red'}">
  <button slot-scope="props" :style="{ color: props.cell.color}">{{props.cell.text}}</button>
</vCell>
</tr>
</table>
```

</docs>
