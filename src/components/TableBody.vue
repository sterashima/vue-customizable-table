<template>
  <tbody>
    <template v-for="row in rows">
      <!-- @slot Each row -->
      <slot :row="row">
        <vRow :row="row" :useExtra="true" v-if="useExtra">
          <template slot-scope="props" slot="extra">
            <!-- @slot extra cell -->
            <slot name="extra-cell" :row="props.row">
              <vCell cell="!! defined by table body !!" />
            </slot>
          </template>
        </vRow>
        <vRow :row="row" v-else/>
      </slot>
    </template>
  </tbody>
</template>

<script lang="ts">
import Vue from 'vue';
import vRow from './Row.vue';
import vCell from './Cell.vue';
export default Vue.extend({
  name: 'vTableBody',
  props : {
    /**
     * Table rows.
     */
    rows: {
      type: Array,
      required: true,
    },
    /**
     * Use extra cell ?
     */
    useExtra: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    vRow,
    vCell,
  },
});
</script>

<style scoped>
tbody tr{
  border-bottom: 1px grey solid;
}
</style>

<docs>
### Examples

The simple table body:

```jsx
<table style="width:100%">
<vTableBody :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]"/>
</table>
```

The simple table body with extra cell:

```jsx
<table style="width:100%">
<vTableBody :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :useExtra="true"/>
</table>
```

The simple table body with custom extra cell:

```jsx
<table style="width:100%">
<vTableBody :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :useExtra="true">
<template slot-scope="props" slot="extra-cell">
  <vCell :cell="`have ${props.row.length} Cells`"/>
</template>
</vTableBody>
</table>
```

Custom content:

```jsx
<table style="width:100%">
<vTableBody :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]">
  <vRow slot-scope="props" :row="props.row.map(cell => `No. ${cell}`)" />
</vTableBody>
</table>
```

Custom content with extra cell:

```jsx
<table style="width:100%">
<vTableBody :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]">
  <vRow slot-scope="props" :row="props.row.map(cell => `No.${cell}`)" :useExtra="true"/>
</vTableBody>
</table>
```

</docs>

