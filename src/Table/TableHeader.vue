<template>
  <thead>
    <transition-group name="fade" tag="tr">
      <th v-if="shouldRenderSelection" key="--th-multi">
        <multi-select :selection="selection" :rows="data" />
      </th>
      <th v-for="(col, idx) in tableColumns"
        :key="col.title || col.field || idx"
        :class="col.thClass" :style="col.thStyle">
        <!-- <th> component (thComp) -->
        <component
          v-if="col.thComp"
          :is="forDynCompIs(col.thComp)"
          :column="col"
          :field="col.field"
          :title="col.title"
          v-bind="$props">
          <head-sort slot="sort" v-if="col.sortable" :field="col.field" :query="query" />
        </component>
        <template v-else>
          {{ col.title }}
           <head-sort v-if="col.sortable" :field="col.field" :query="query" />
        </template>

        <i v-if="col.explain" class="fa fa-info-circle" style="cursor: help" :title="col.explain"></i>

      </th>
    </transition-group>
  </thead>
</template>
<script>
import HeadSort from './HeadSort.vue'
import MultiSelect from './MultiSelect.vue'
import props from '../_mixins/props'
import shouldRenderSelection from '../_mixins/shouldRenderSelection'

export default {
  name: 'TableHeader',
  components: { HeadSort, MultiSelect },
  mixins: [props, shouldRenderSelection],
  computed: {
    tableColumns() {
      if(this.leftFixed){
        return this.columns.filter(col => col.fixed === true || col.fixed === 'left' );
      } else if (this.rightFixed){
        return this.columns.filter(col => col.fixed === 'right' );
      } else {
        return this.columns.filter(col => col.fixed !== true && col.fixed !== 'left' && col.fixed !== 'right');
      }
    }
  }
}
</script>
