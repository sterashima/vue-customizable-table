<template>
<vRow :row="labels" :useExtra="extraColName != null">
  <template slot-scope="props">
    <vHeaderCell :cell="props.cell" v-if="sort == undefined" />
    <vSortCell :label="props.cell" :isSort="props.cell == sort.label" :isAsc="sort.isAsc" v-else/>
  </template>
  <vHeaderCell :cell="extraColName" v-if="extraColName != null" slot="extra" />
</vRow>
</template>

<script lang="ts">
interface Sort {
  label: string;
  isAsc: boolean;
}
import Vue from 'vue';
import vRow from './Row.vue';
import vHeaderCell from './HeaderCell.vue';
import vSortCell from './SortCell.vue';
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
    /**
     * Sort
     */
    sort: {
      type: Object as () => Sort,
    },
  },
  components: {
    vHeaderCell, vRow, vSortCell,
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

Sort header:

```jsx
<table style="width:100%">
<vSimpleHeader :labels="[1,2,3,4,5,6,7]" extraColName="Action" :sort="{label: '1', isAsc: true}"/>
</table>
```

Custom sort cell:

```jsx
<table style="width:100%">
<vSimpleHeader :labels="[1,2,3,4,5,6,7]" extraColName="Action" :sort="{label: '1', isAsc: true}"/>
</table>
```

</docs>

