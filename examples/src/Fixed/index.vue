<template>
  <div style="margin: 0 auto; width: 100%">
    <code>query: {{ query }}</code>
    <datatable v-bind="$data" />
  </div>
</template>
<script>
import mockData from '../_mockData'

export default {
  data: () => ({
    fixHeaderAndSetBodyMaxHeight: 800,
    tblStyle: 'table-layout: fixed', // must
    tblClass: 'table-bordered',
    columns: [
      { title: 'User ID', field: 'uid', sortable: true, fixed: true },
      { title: 'Username', field: 'name' },
      { title: 'Age', field: 'age', sortable: true },
      { title: 'Email', field: 'email' },
      { title: 'Country', field: 'country'}
    ].map(col => (col.colStyle = { width: '200px' }, col)),
    data: [],
    summary: {},
    total: 0,
    query: {},
    fullHeight: {
      enabled: true,
      heightCorrection: -350
    },
    selection: [],
  }),
  watch: {
    query: {
      handler (query) {
        mockData(query).then(({ rows, total, summary }) => {
          this.data = rows
          this.total = total
          this.summary = summary
        })
      },
      deep: true
    }
  }
}
</script>
