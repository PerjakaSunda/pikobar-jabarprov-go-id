<template>
  <div class="overflow-x-auto">
    <table class="w-full text-left">
      <thead>
        <tr>
          <th class="bg-gray-300 uppercase text-gray-600 text-sm px-4 py-2 border-t-2 border-b-2 border-solid border-gray-400">
            No.
          </th>
          <th
            v-for="col in columns"
            :key="col.prop"
            class="bg-gray-300 uppercase text-gray-600 text-sm px-4 py-2 border-t-2 border-b-2 border-solid border-gray-400"
          >
            <b>
              {{ col.label }}
            </b>
          </th>
        </tr>
      </thead>
      <tbody>
        <template v-if="tableData && tableData.length">
          <tr v-for="(row, rowIndex) in tableData" :key="rowIndex">
            <td class="px-4 py-2 text-gray-800 border-b-2 border-solid border-gray-300">
              {{ rowIndex + 1 }}
            </td>
            <td
              v-for="(col, colIndex) in columns"
              :key="col.prop"
              class="px-4 py-2 text-gray-800 border-b-2 border-solid border-gray-300"
            >
              {{ getCellValue(col, row, colIndex, rowIndex) }}
            </td>
          </tr>
        </template>
        <template v-else>
          <tr>
            <td
              :colspan="columns.length + 1"
            >
              <div
                class="w-full p-5"
                style="min-height: 300px;"
              >
                <ContentLoader
                  class="w-full lg:hidden"
                  :speed="3"
                  :width="400"
                  :height="200"
                  primary-color="#eee"
                  secondary-color="#fff"
                >
                  <rect
                    v-for="i in 6"
                    :key="i"
                    x="0"
                    :y="((i - 1) * 36)"
                    width="100%"
                    height="18"
                    rx="3"
                    ry="3"
                  />
                </ContentLoader>
                <ContentLoader
                  class="w-full hidden lg:block"
                  :speed="3"
                  :width="400"
                  :height="100"
                  primary-color="#eee"
                  secondary-color="#fff"
                >
                  <rect
                    v-for="i in 6"
                    :key="i"
                    x="0"
                    :y="((i - 1) * 16)"
                    width="100%"
                    height="8"
                    rx="3"
                    ry="3"
                  />
                </ContentLoader>
              </div>
            </td>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ContentLoader } from 'vue-content-loader'
import { getLogistics } from '../../../api/donation'

// const NAMES = [
//   'Masker N95',
//   'Masker Bedah',
//   'Cap',
//   'Pelindung Sepatu',
//   'Sarung Tangan non-Steril',
//   'Boots',
//   'Kantong Jenazah',
//   'Apron'
// ]
// const getRandomName = () => NAMES[Math.floor(Math.random() * NAMES.length)]
export default {
  components: {
    ContentLoader
  },
  data () {
    return {
      columns: [
        {
          prop: 'name',
          label: 'Kebutuhan Logistik'
        },
        {
          prop: 'needed',
          label: 'Jumlah Dibutuhkan'
        },
        {
          prop: 'fulfilled',
          label: 'Jumlah Terpenuhi'
        }
      ],
      tableData: null
    }
  },
  mounted () {
    this.getTableData()
  },
  methods: {
    getCellValue (column, row, columnIndex, rowIndex) {
      return row[column.prop]
    },
    getTableData () {
      this.tableData = null
      return getLogistics().then(({ data }) => {
        console.log(data)
      })
    }
  }
}
</script>

<style>

</style>
