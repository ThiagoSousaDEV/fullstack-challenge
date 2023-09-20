<template>
  <header>
    <section class="container col-md-10">
      <form id="data-form" @submit="createData">
        <div class="row">
          <div class="col-md-3 form-group">
            <input
              type="text"
              name="firstname"
              v-model="firstname"
              placeholder="First Name"
              class="form-control"
              required
            />
          </div>
          <div class="col-md-3 form-group">
            <input
              type="text"
              name="lastname"
              v-model="lastname"
              placeholder="Last Name"
              class="form-control"
              required
            />
          </div>
          <div class="col-md-3 form-group">
            <input
              type="number"
              name="participation"
              v-model="participation"
              placeholder="Participation %"
              class="form-control"
              required
            />
          </div>
          <div class="col-md-1 form-group">
            <input type="submit" class="button" value="Send" />
          </div>
        </div>
      </form>
    </section>
  </header>
</template>

<script>
import axios from "axios";
export default {
  name: "Header",

  data() {
    return {
      firstname: null,
      lastname: null,
      participation: null,
    };
  },
  methods: {
    async createData(e) {
      e.preventDefault();
      const data = {
        firstname: this.firstname,
        lastname: this.lastname,
        participation: this.participation,
      };

      try {
        const response = await axios.post("http://localhost:3000/values", data, {
          headers: { "Content-Type": "application/json" },
        });
        
        setTimeout(function () {
          window.location.reload();
        });
      } catch (error) {
        console.error("Erro ao fazer a solicitação POST:", error);
      }
    },
  },
};
</script>

<style scoped>
header {
  width: 100%;
  background-color: #00b8e2;
  padding: 4rem;
}

input {
  padding: 20px;
  border: transparent;
  border-radius: 4px;
  color: #5c5c5c;
  margin-bottom: 10px;
}

.form-control:focus {
  box-shadow: 0 0 0 0;
  outline: 0;
}

.button {
  background-color: transparent;
  color: #fff;
  border: 2px solid white;
  padding: 18px 40px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  text-transform: uppercase;
}

.button:hover {
  background-color: white;
  color: #00b8e2;
  transition: 0.6s;
  padding: 18px 40px;
  border-radius: 4px;
  cursor: pointer;
}

@media (min-width: 768px) {
  .col-md-3 {
    width: 28%;
  }
}
</style>
