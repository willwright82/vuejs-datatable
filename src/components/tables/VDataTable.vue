<template>
  <table class="ui striped very padded table">
    <thead>
      <tr>
        <th
          v-for="column in columns"
          :key="column.dataKey"
          :class="column.align ? column.align + ' ' + 'aligned' : ''"
          >
          {{ column.name }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="campaign in display_rows()" :key="campaign.code">
        <td
          v-for="column in columns"
          :key="column.dataKey"
          :class="column.align ? column.align + ' ' + 'aligned' : ''"
          >
          {{
          column.formatValue ?
          formatValue(column, campaign[column.dataKey])
          : campaign[column.dataKey]
          }}
        </td>
      </tr>
      <tr
        v-for="empty_row in (perPage - display_rows().length)"
        :key="empty_row"
      >
        <td
          v-for="column in columns"
          :key="column.dataKey"
          >
          &nbsp;
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th colspan="3">
          <div class="ui pagination menu">
            <a
              class="icon item"
              :class="currentPage <= 0 ? 'disabled' : ''"
              @click="currentPage > 0 ? currentPage -= 1 : ''"
            >
              <i class="left chevron icon"></i>
            </a>
            <a
              v-for="(page, index) in pages"
              :key="index"
              class="item"
              :class="index === currentPage ? 'active' : ''"
              @click="currentPage = index"
              >
              {{ index+1 }}
            </a>
            <a
              class="icon item"
              :class="currentPage >= pages - 1 ? 'disabled' : ''"
              @click="currentPage < pages - 1 ? currentPage += 1 : ''"
            >
              <i class="right chevron icon"></i>
            </a>
          </div>
          &nbsp;
          {{ pages }} pages
          ({{ rows.length }} results)
        </th>
      </tr>
    </tfoot>
  </table>
</template>

<script>

export default {
  name: 'v-data-table',
  props: {
    columns: Array,
    rows: Array,
    perPage: Number,
  },
  data() {
    return {
      currentPage: 0,
    }
  },
  methods: {
    formatValue(column, value) {
      return column.formatValue(value)
    },
    display_rows() {
      var start = (this.currentPage) * this.perPage
      var end = start + this.perPage
      return this.rows.slice(start, end)
    },
    change_page(page) {
      this.currentPage = page
    },
  },
  computed: {
    pages() {
      return Math.ceil(this.rows.length/this.perPage)
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ui.table {
  border-radius: 0;
  background-color: #F6F7F8;
}

.ui.table,
.ui.table thead th,
.ui.table tfoot th,
.ui.table tr td {
  border-radius: 0 !important;
  border-color: transparent !important;
}

.ui.striped.table tbody tr,
.ui.striped.table>tr {
  background-color: #E6EBEC;
}

.ui.striped.table tbody tr:nth-child(2n),
.ui.striped.table>tr:nth-child(2n) {
  background-color: #F6F7F8;
}

.ui.table thead th,
.ui.table tfoot th {
  background: transparent;
}

.ui.table tfoot th {
  padding-top: 3em;
  padding-left: 0;
}

.ui.table thead th, table tr td {
  color: #738799 !important;
}

.ui.table tr td:nth-child(1) {
  text-transform: uppercase;
  font-weight: bold;
  color: #446B92 !important;
}

.ui.table tfoot tr th {
  color: #999 !important;
}
</style>
