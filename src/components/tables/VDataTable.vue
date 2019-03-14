<template>
  <table class="ui striped very padded table">
    <thead>
      <tr>
        <th
          v-for="column in columns"
          :key="column.dataKey"
          :class="[column.align, 'aligned']"
        >
          {{ column.name }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="campaign in displayRows" :key="campaign.code">
        <td
          v-for="column in columns"
          :key="column.dataKey"
          :class="[column.align, 'aligned']"
        >
          {{ formatValue(column, campaign[column.dataKey]) }}
        </td>
      </tr>
      <tr
        v-for="emptyRow in (perPage - displayRows.length)"
        :key="emptyRow"
      >
        <td>&nbsp;</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th colspan="3">
          <div class="ui pagination menu">
            <a
              class="icon item"
              :class="{ 'disabled': currentPage <= 0 }"
              @click="currentPage = Math.max(currentPage - 1, 0)"
            >
              <i class="left chevron icon" />
            </a>
            <a
              v-for="(page, index) in pages"
              :key="index"
              class="item"
              :class="{ 'active': index === currentPage }"
              @click="currentPage = index"
              >
              {{ index+1 }}
            </a>
            <a
              class="icon item"
              :class="{ 'disabled': currentPage >= pages - 1 }"
              @click="currentPage = Math.min(pages - 1, currentPage + 1)"
            >
              <i class="right chevron icon" />
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
  name: 'VDataTable',
  props: {
    columns: {
      type: Array,
      required: true,
    },
    rows: {
      type: Array,
      required: true,
    },
    perPage: {
      type: Number,
      default: 10,
    },
  },
  data() {
    return {
      currentPage: 0,
    };
  },
  methods: {
    formatValue(column, value) {
      if (column.formatValue) {
        return column.formatValue(value);
      }
      return value;
    },
  },
  computed: {
    pages() {
      return Math.ceil(this.rows.length / this.perPage);
    },
    displayRows() {
      var start = this.currentPage * this.perPage;
      var end = start + this.perPage;
      return this.rows.slice(start, end);
    },
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
