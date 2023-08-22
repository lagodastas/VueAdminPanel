<template>
  <div>
    <h2>Implement jQuery DataTable in Vue Js</h2>
    <table class="table" id="datatable">
      <thead>
      <tr>
        <th>ID</th>
        <th>Product Title</th>
        <th>Product Price</th>
        <th>Created On</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in products" :key="item.DT_RowId">
        <td>{{ item.DT_RowId }}</td>
        <td>{{ item.colors.id }}</td>
        <td>{{ item.colors.name }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import "jquery/dist/jquery.min.js";
import "bootstrap/dist/css/bootstrap.min.css";
import "datatables.net-dt/js/dataTables.dataTables";
import "datatables.net-dt/css/jquery.dataTables.min.css";
import axios from "axios";
import $ from "jquery";
export default {
  mounted() {
    axios.get("/colors.json").then((response) => {
      this.products = response.data.data;
      // console.log('this.products');
      // console.log(this.products);
      $("#datatable").DataTable();
    });
  },
  data: function () {
    return {
      products: [],
    };
  },
};
</script>
