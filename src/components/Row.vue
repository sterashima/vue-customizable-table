<template>
  <tr>
    <template v-for="cell in row">
      <!-- @slot Each cell -->
      <slot :cell="cell" :row="row">
        <vCell :cell="cell" />
      </slot>
    </template>
    <!-- @slot Extra cell -->
    <slot :row="row" name="extra" v-if="useExtra">
      <vCell :cell="row" />
    </slot>
  </tr>
</template>

<script long="ts">
import Vue from 'vue';
import vCell from './Cell';
export default Vue.extend({
  name: 'vRow',
  props: {
    /**
     * Array of Cells
     */
    row: {
      type: Array,
      required: true,
    },
    /**
     * Use Extra Cell ?
     */
    useExtra: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    vCell,
  },
});
</script>
<style scoped>
tr {
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  transition: all 0.3s ease;
}

tbody > tr:nth-child(odd) > td {
  background-color: #f5f5f5;
}
tbody > tr:hover > td {
  background-color: rgba(0, 0, 0, 0.12);
}
</style>

<docs>
### Examples

The simple row:

```jsx
<table style="width:100%">
<vRow :row="[1,2,3,4,5,6,7]"/>
</table>
```

Custom content:

```jsx
<table style="width:100%">
<vRow :row="[1,2,3,4,5,6,7]">
  <vCell slot-scope="props" :cell="`No. ${props.cell}`" />
</vRow>
</table>
```

With extra cell:

```jsx
<table style="width:100%">
<vRow :row="[1,2,3,4,5,6,7]" :useExtra="true" />
</table>
```

With custom extra cell:

```jsx
<table style="width:100%">
<vRow :row="[1,2,3,4,5,6,7]" :useExtra="true" >
  <vCell slot-scope="props" :cell="`No. ${props.cell}`"/>
  <vCell slot-scope="props" slot="extra" :cell="`have ${props.row.length} Cells`"/>
</vRow>
</table>
```


</docs>
