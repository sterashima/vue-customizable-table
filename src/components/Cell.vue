<template>
  <td>
    <!-- @slot Contents of cell -->
    <slot :cell="cell">
      {{ cell }}
    </slot>
  </td>
</template>

<script lang="ts">
/**
 * The table cell
 */
import Vue from 'vue';
export default Vue.extend({
  name: 'vCell',
  props: {
    /**
     * Value of Cell
     */
    cell: {},
   },
});
</script>

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
