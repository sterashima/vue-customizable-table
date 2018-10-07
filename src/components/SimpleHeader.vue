<template>
<vRow :row="labels" :useExtra="extraColName != null">
  <vHeaderCell slot-scope="props" :cell="props.cell"/>
  <vHeaderCell :cell="extraColName" v-if="extraColName != null" slot="extra" />
</vRow>
</template>

<script lang="ts">
import Vue from 'vue';
import vRow from './Row.vue';
import vHeaderCell from './HeaderCell.vue';
export default {
  name: 'vSimpleHeader',
  props: {
    /**
     * Extra colunm name
     */
    extraColName: {
      type: String,
      default: null,
    },
    /**
     * Colunm names
     */
    labels: {
      type: Array,
      required: true,
    },
  },
  components: {
    vHeaderCell, vRow,
  },
};
</script>

<style>

</style>

<docs>
### Examples

The simple header:

```jsx
<table style="width:100%">
<vSimpleHeader :labels="[1,2,3,4,5,6,7]"/>
</table>
```

With extra cell:

```jsx
<table style="width:100%">
<vSimpleHeader :labels="[1,2,3,4,5,6,7]" extraColName="Action" />
</table>
```
</docs>

