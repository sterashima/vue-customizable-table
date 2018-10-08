<template>
<div class="pager-wrapper">
  <span @click.prevent="previous">
    <!-- @slot Previous page button -->
    <slot name="prevPage" :disabled="value == 0 || disabled" @click.prevent="previous">
      <button class="page-button" :disabled="value == 0 || disabled">&lt;</button>
    </slot>
  </span>

  <template v-for="(item, index) in items">
    <span v-if="isNaN(item)" :key="index">{{item}}</span>
    <span @click="$emit('input', item - 1)" :key="index" v-else>
      <!-- @slot Page button -->
      <slot :page="item" :active="value==(item - 1)">
        <button class="page-button" :class="{active: value==(item - 1)}" :disabled="disabled || value==(item - 1)">{{item}}</button>
      </slot>
    </span>
  </template>

  <span @click.prevent="next">
    <!-- @slot Next page button -->
    <slot name="nextPage" :disabled="value == (length - 1) || disabled">
      <button class="page-button" :disabled="value == (length - 1) || disabled">&gt;</button>
    </slot>
  </span>
</div>
</template>

<style scoped>
.pager-wrapper {
  width: 100%;
  max-width:100%;
  text-align: center;
}

.pager-wrapper .page-button{
  display: inline-block;
  text-decoration: none;
  text-align: center;
  cursor: pointer;
  background-color: white;
  border: 1px solid black;
  text-decoration: none;
  transition: background-color .3s;
  border: 1px solid #ddd;
  margin: 0 2px;
  box-shadow: 0px 2px 4px -1px rgba(0,0,0,0.2), 0px 4px 5px 0px rgba(0,0,0,0.14), 0px 1px 10px 0px rgba(0,0,0,0.12);
  height: 30px;
  width:30px;
}

.pager-wrapper .page-button.active{
  background: #1867c0 ;
  cursor: auto;
  color: white;
}
</style>

<script lang="ts">
/**
 * The pager
 */
import Vue from 'vue';
export default Vue.extend({
  name: 'vPager',
  props : {
    /**
     * disabled
     */
    disabled: Boolean,

    /**
     * Number of pages
     */
    length: {
      type: Number,
      default: 0,
      validator: (val: number): boolean => val % 1 === 0,
    },

    /**
     * Number of display items
     */
    visibleButtons: {
      type: Number,
      default: 10,
    },

    /**
     * Current page
     * @model
     */
    value: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    items(): Array<string | number> {
      const maxLength = this.visibleButtons;
      if (this.length <= maxLength) {
        return this.range(1, this.length);
      }

      const even = maxLength % 2 === 0 ? 1 : 0;
      const left = Math.floor(maxLength / 2);
      const right = this.length - left + 1 + even;
      if (this.value > left && this.value < right) {
        const start = this.value - left + 2;
        const end = this.value + left - 2 - even;
        return [1, '...', ...this.range(start, end), '...', this.length];

      } else if (this.value === left) {
        const end = this.value + left - 1 - even;
        return [...this.range(1, end), '...', this.length];

      } else if (this.value === right) {
        const start = this.value - left + 1;
        return [1, '...', ...this.range(start, this.length)];

      } else {
        return [
          ...this.range(1, left),
          '...',
          ...this.range(right, this.length),
        ];
      }
    },
  },
  methods: {
    isNaN(v: (string | number)): boolean {
      return isNaN(Number(v) );
    },
    next(): void {
      if ((this.value + 1) === this.length) {
        return;
      }
      this.$emit('input', this.value + 1 );
      this.$emit('next');
    },
    previous(): void {
      if (this.value === 0 ) {
        return;
      }
      this.$emit('input', this.value - 1 );
      this.$emit('previous');
    },
    range(from: number, to: number ): number[] {
      const range: number[] = [];
      from = from > 0 ? from : 1;
      for (let i = from; i <= to; i++) {
        range.push(i);
      }
      return range;
    },
  },
});
</script>

<docs>
### Examples

The simple pager:

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `<vPager v-model="page" :length="maxPage"/>`
})
```

Custom pagenation button: 

```js
new Vue({
  data(){
    return {
      page: 0,
      maxPage: 20
    }
  },
  template: `
    <vPager v-model="page" :length="maxPage">
      <button slot="prevPage" slot-scope="props" :disabled="props.disabled">Prev</button>
      <button slot-scope="props" :disabled="props.active">{{props.page}}</button>
      <button slot="nextPage" slot-scope="props" :disabled="props.disabled">Next</button>
    </vPager>
  `
})
```
</docs>
