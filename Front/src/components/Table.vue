<template>
  <section class="container">
    <div class="row" v-if="values.length > 0">
      <div
        class="table-responsive-sm col-lg-6"
        style="display: grid; align-items: center"
      >
        <table>
          <thead>
            <tr>
              <th scope="col"></th>
              <th scope="col">First Name</th>
              <th scope="col">Last Name</th>
              <th scope="col" style="text-align: center">Participation</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="value in values" :key="value.id" scope="row">
              <td style="text-align: center">{{ value.id }}</td>
              <td>{{ value.firstname }}</td>
              <td>{{ value.lastname }}</td>
              <td style="text-align: center">{{ value.participation }}%</td>
              <td style="text-align: center">
                <button class="delete-btn" @click="deleteValue(value.id)">
                  <i class="bi bi-trash3"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="col-lg-2"></div>
      <div class="col-lg-4">
        <div><Chart /></div>
      </div>
    </div>
    <div v-else>
      <p style="text-align: center">There is no data to display</p>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import Chart from "./Chart.vue";

export default {
  name: "Table",
  components: {
    Chart,
  },

  data() {
    return {
      values: [],
      value: null,
    };
  },

  methods: {
    async getValues() {
      try {
        const response = await axios.get("http://localhost:3000/values");
        this.values = response.data;
        console.log(response.data);
      } catch (error) {
        console.error("Error when fetching values:", error);
      }
    },
    async deleteValue(id) {
      try {
        await axios.delete(`http://localhost:3000/values/${id}`);
        setTimeout(function () {
          window.location.reload();
        }, 0);
      } catch (error) {
        console.error("Error deleting value:", error);
      }
    },
  },
  mounted() {
    this.getValues();
  },
};
</script>


<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
}

table thead tr th {
  font-size: 14px;
  text-align: left;
  font-weight: 600;
  letter-spacing: 0.35px;
  color: #707070;
  opacity: 1;
  padding: 12px;
  vertical-align: top;
  border: 1px solid #b5b5b5;
}

table tbody tr td {
  font-size: 14px;
  text-align: left;
  letter-spacing: 0.35px;
  font-weight: normal;
  color: #707070;
  background-color: transparent;
  padding: 8px;
  border: 1px solid #b5b5b5;
}

button.delete-btn {
  border-color: transparent;
  background-color: transparent;
  color: #707070;
}

button.delete-btn:hover {
  color: #d10606;
  transition: 0.6s;
}
</style>
