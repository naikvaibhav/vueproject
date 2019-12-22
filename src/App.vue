<template>
  <div id="app" class="small-container">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <!-- <Search /> -->
    <h1>Employee's</h1>
    <!--Retrieving events from the child-->
    <!-- <component @name-of-the-emitted-event="methodToCallOnceEmitted"></component> -->
    <employee-form @add:employee="addEmployee" />

    <employee-table
      v-bind:employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
// import Search from "./components/Search.vue";
import EmployeeTable from "@/components/EmployeeTable.vue";
import EmployeeForm from "@/components/EmployeeForm.vue";
import axios from "axios";
export default {
  name: "app",
  components: {
    EmployeeTable,
    EmployeeForm
    // HelloWorld,
    // Search
  },
  data() {
    return {
      employees: [
        // {
        //   id: 1,
        //   name: "Richard Hendricks",
        //   email: "richard@piedpiper.com"
        // },
        // {
        //   id: 2,
        //   name: "Bertram Gilfoyle",
        //   email: "gilfoyle@piedpiper.com"
        // },
        // {
        //   id: 3,
        //   name: "Dinesh Chugtai",
        //   email: "dinesh@piedpiper.com"
        // }
      ]
    };
  },
  mounted() {
    this.getEmployees();
  },
  methods: {
    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };
      this.employees = [...this.employees, newEmployee];
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(employee => employee.id !== id);
    },
    editEmployee(id, updatedEmployee) {
      this.employees = this.employees.map(employee =>
        employee.id === id ? updatedEmployee : employee
      );
    },
    async getEmployees() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/users"
        );
        const data = response.data;
        this.employees = data;
      } catch (error) {
        window.console.error(error);
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
h1 {
  text-align: left;
}
</style>
