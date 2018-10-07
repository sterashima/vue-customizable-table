<template>
<div style="width:100%">
  <!-- @slot pager -->
  <slot name="pager" :page="page" :length="pageLength">
    <vPager :value="page" @input="$emit('page')" :length="pageLength"/>
  </slot>
  
  
  <table>
    <vTableHeader :header="header" v-if="header" :extraColName="extraColName" @filter="$emit('filter', $event)"/>
    <vTableBody :rows="rows" :useExtra="isExtra" v-if="$scopedSlots['action-cell']">
      <!-- @slot action cell -->
      <template slot="extra-cell" slot-scope="props">
        <slot name="action-cell" :row="props.row"></slot>
      </template>

      <!-- @slot row -->
      <template slot-scope="props" v-if="$scopedSlots['default']">
        <slot slot-scope="props" :row="props.row"></slot>
      </template>
    </vTableBody>

    <vTableBody :rows="rows" :useExtra="isExtra" v-else>
      <!-- @slot row -->
      <template slot-scope="props" v-if="$scopedSlots['default']">
        <slot slot-scope="props" :row="props.row"></slot>
      </template>
    </vTableBody>
  </table>

  <!-- @slot pager -->
  <slot name="pager" :page="page" :length="pageLength">
    <vPager :value="page" @input="$emit('page')" :length="pageLength"/>
  </slot>
</div>
</template>

<script lang="ts">
/**
 * The table
 */
import Vue from 'vue';
import vTableBody from './TableBody.vue';
import vTableHeader from './TableHeader.vue';
import vPager from './Pager.vue' ;
export default Vue.extend({
  name : 'vTable',
  components: {
    vTableBody , vTableHeader , vPager,
  },
  computed: {
    isExtra(): boolean {
      if (this.useExtra) {
        return true;
      }
      if (this.extraColName != null) {
        return true;
      }
      return false;
    },
  },
  props: {
    /**
     * Table rows.
     */
    rows: {
      type: Array,
      required: true,
    },
    useExtra: {
      type: Boolean,
      default: false,
    },
    header: {
      type: Array,
    },
    extraColName: {
      type: String,
      default: null,
    },
    pageLength: Number,
    page : {
      type: Number,
      default: 0,
    },
  },
});
</script>


<style scoped>
table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 2rem;
  margin-top: 2rem;
  background-color: #fff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.12), 0 1px 2px 0 rgba(0, 0, 0, 0.24);
}
</style>

<docs>
### Examples

The simple table:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" :pageLength="10"/>
```

The simple table with coustom pager:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :pageLength="10">
  <vPager slot="pager" slot-scope="prop" :value="prop.page" @input="prop.page = $event" :length="prop.length">
    <button slot="prevPage" slot-scope="props" :disabled="props.disabled">Prev</button>
    <button slot-scope="props" :disabled="props.active">{{props.page}}</button>
    <button slot="nextPage" slot-scope="props" :disabled="props.disabled">Next</button>
  </vPager>
</vTable>
```

The simple table without header:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :pageLength="10"/>
```

The simple table with extra cell:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" extraColName="Extra" :pageLength="10"/>
```

The simple table with extra cell without header:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :useExtra="true" :pageLength="10"/>
```

The simple table with custom extra cell:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" extraColName="Extra" :pageLength="10">
<template slot-scope="props" slot="action-cell">
  <vCell :cell="`have ${props.row.length} Cells`"/>
</template>
</vTable>
```

Custom row:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" :pageLength="10">
  <vRow slot-scope="props" :row="props.row.map(cell => `No. ${cell}`)"/>
</vTable>
```

Custom content with extra cell:

```jsx
<vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" extraColName="Extra" :pageLength="10" :page="5">
  <vRow slot-scope="props" :row="props.row.map(cell => `No.${cell}`)" :useExtra="true"/>
</vTable>
```

</docs>

