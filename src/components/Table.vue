<template>
<div style="width:100%">
  <!-- @slot pager -->
  <slot name="pager" :page="page" :length="pageLength">
    <vPager :value="page" @input="$emit('page', $event)" :length="pageLength"/>
  </slot>
  
  <table>
    <vTableHeader :header="header" v-if="header" :extraColName="extraColName" @input="$emit('filter', $event)" :value="filter">
      <template slot="filter" slot-scope="props" v-if="$scopedSlots['filter-cell']">
        <!-- @slot filter cell -->
        <slot name="filter-cell" :cell="props.cell"></slot>
      </template>
    </vTableHeader>

    <vTableBody :rows="rows" :useExtra="isExtra" v-if="$scopedSlots['action-cell']">
      <!-- @slot action cell -->
      <template slot="extra-cell" slot-scope="props">
        <slot name="action-cell" :row="props.row"></slot>
      </template>

      <template slot-scope="props" v-if="$scopedSlots['default']">
        <!-- @slot row -->
        <slot slot-scope="props" :row="props.row"></slot>
      </template>
    </vTableBody>

    <vTableBody :rows="rows" :useExtra="isExtra" v-else>
      
      <template slot-scope="props" v-if="$scopedSlots['default']">
        <!-- @slot row -->
        <slot slot-scope="props" :row="props.row"></slot>
      </template>
    </vTableBody>
  </table>

  <!-- @slot pager -->
  <slot name="pager" :page="page" :length="pageLength">
    <vPager :value="page" @input="$emit('page', $event)" :length="pageLength"/>
  </slot>
</div>
</template>

<script lang="ts">
interface Header {
  label: string;
  filter: string;
}
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
    /**
     * Extra col ?
     */
    useExtra: {
      type: Boolean,
      default: false,
    },
    /**
     * table header
     */
    header: {
      type: Array as () => Header[],
    },
    /**
     * table filter
     */
    filter: {
      default: () => ({}),
    },
    /**
     * colname of extra col.
     */
    extraColName: {
      type: String,
      default: null,
    },
    /**
     * Max page length
     */
    pageLength: Number,
    /**
     * Current page
     */
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

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" :pageLength="maxPage" :page="page" @page="page = $event"/>
  `
})
```

The simple table with coustom pager:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']">
      <vPager slot="pager" slot-scope="prop" v-model="page" :length="maxPage">
        <button slot="prevPage" slot-scope="props" :disabled="props.disabled">Prev</button>
        <button slot-scope="props" :disabled="props.active">{{props.page}}</button>
        <button slot="nextPage" slot-scope="props" :disabled="props.disabled">Next</button>
      </vPager>
    </vTable>
  `
})
```

The simple table without header:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :pageLength="maxPage" :page="page" @page="page = $event"/>
  `
})
```

The simple table with extra cell:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" extraColName="Extra" :pageLength="maxPage" :page="page" @page="page = $event"/>
  `
})
```

The simple table with extra cell without header:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :useExtra="true" :pageLength="maxPage" :page="page" @page="page = $event"/>
  `
})
```

The simple table with custom extra cell:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :useExtra="true" :pageLength="maxPage" :page="page" @page="page = $event">
      <template slot-scope="props" slot="action-cell">
        <vCell :cell="\`have \${props.row.length} Cells\`"/>
      </template>
    </vTable>
  `
})
```

Custom row:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" :pageLength="maxPage" :page="page" @page="page = $event">
      <vRow slot-scope="props" :row="props.row.map(cell => \`No. \${cell}\`)"/>
    </vTable>
  `
})
```

Custom content with extra cell:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vTable :rows="[[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]]" :header="['one', 'two', 'three', 'four', 'five']" extraColName="Extra" :pageLength="maxPage" :page="page" @page="page = $event">
      <vRow slot-scope="props" :row="props.row.map(cell => \`No. \${cell}\`)" :useExtra="true"/>
    </vTable>
  `
})
```

Custom filter:

```vue
<template>
<div>
  <vTable :rows="rows" :filter="filter" :header="header" :page="page" :pageLength="20" @page="page = $event" @filter="filter = $event">
    <template slot-scope="props" slot="filter-cell">
      <select v-if="props.cell.options" v-model="props.cell.filter">
        <option value=""></option>
        <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
      </select>
      <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter" type="date">
      <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter" type="number">
      <input v-else v-model="props.cell.filter" style="width:100%;">
    </template>
  </vTable>
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
      ],
      rows: [[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]],
      page: 0
    };
  },
}
</script>
```

Custom table:

```vue
<template>
<div>
  <vTable :rows="rows" :filter="filter" :header="header" @filter="filter = $event" extraColName="Action" >
    <template slot-scope="props" slot="filter-cell">
      <select v-if="props.cell.options" v-model="props.cell.filter">
        <option value=""></option>
        <option :key="option" :value="option" v-for="option in props.cell.options">{{option}}</option>
      </select>
      <input v-else-if="props.cell.type == 'date'" v-model="props.cell.filter" type="date">
      <input v-else-if="props.cell.type == 'number'" v-model="props.cell.filter" type="number">
      <input v-else v-model="props.cell.filter" style="width:100%;">
    </template>

    <vPager slot="pager" slot-scope="prop" v-model="page" :length="20">
        <button slot="prevPage" slot-scope="props" :disabled="props.disabled">Prev</button>
        <button slot-scope="props" :disabled="props.active">{{props.page}}</button>
        <button slot="nextPage" slot-scope="props" :disabled="props.disabled">Next</button>
    </vPager>

    <vCell slot-scope="props" slot="action-cell">
      <button>Action</button>
    </vCell>

  </vTable>
<div>page: {{page}}</div>
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
      ],
      rows: [[1,2,3,4,5],[1,2,3,4,5],[1,2,3,4,5]],
      page: 0
    };
  },
}
</script>
```
</docs>

