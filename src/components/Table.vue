<template>
  <b-row>
    <b-col md="12">
      <div class="overflow-auto">
        <b-table
          id="my-table"
          :items="data"
          :per-page="perPage"
          :current-page="currentPage"
          small
        ></b-table>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
        ></b-pagination>
      </div>
    </b-col>
  </b-row>
</template>
<script>
import { BCol, BRow } from "bootstrap-vue";
export default {
  name: "App",
  components: {
    BCol,
    BRow,
  },
  computed: {
    rows() {
      return this.data.length;
    },
  },
  props: {
    data: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      perPage: 10,
      currentPage: 1,
    };
  },
};
</script>
