<template>
  <th>
    <!-- @slot Contents of cell -->
    <slot :cell="cell">
      {{ cell }}
    </slot>
  </th>
</template>

<script lang="ts">
/**
 * The table header rcell
 */
import Vue from 'vue';
export default Vue.extend({
  name: 'vHeaderCell',
  props : {
    /**
     * Value of Cell
     */
    cell: {},
  },
});
</script>

<style>

</style>

<docs>
### Examples

The simple cell:

```jsx
<table style="width:100%">
<tr>
<vHeaderCell cell="Header Cell Value"/>
</tr>
</table>
```

Custom content:

```jsx
<table style="width:100%">
<tr>
<vHeaderCell :cell="{text: 'red', color: 'red'}">
  <button slot-scope="props" :style="{ color: props.cell.color}">{{props.cell.text}}</button>
</vHeaderCell>
</tr>
</table>
```

</docs>
