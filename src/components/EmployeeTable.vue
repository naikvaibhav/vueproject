<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No Employees</p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-bind:key="employee.id" v-for="employee in employees">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">
              Cancel
            </button>
          </td>
          <td v-else>
            <button @click="editMode(employee)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">
              Delete
            </button>
          </td>
          <!-- <td>{{ employee.name }}</td>
          <td>{{ employee.email }}</td>
          <button @click="editMode(employee.id)">Edit</button>
          <button @click="$emit('delete:employee', employee.id)">Delete</button> -->
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(employee) {
      this.cachedEmployee = Object.assign({}, employee);
      this.editing = employee.id;
    },
    editEmployee(employee) {
      if (employee.name === " " || employee.email === " ") return;
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    },
    cancelEdit(employee) {
      Object.assign(employee, this.cachedEmployee);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

.action {
  margin-right: 10%;
}
</style>
