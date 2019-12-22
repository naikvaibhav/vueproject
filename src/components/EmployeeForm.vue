<template>
  <div id="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @keypress="clearStatus"
        @focus="clearStatus"
      />
      <label>Employee email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        !Please fill out all the required details
      </p>
      <p v-if="success" class="success-message">Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }
      // Emitting events to the parent
      // this.$emit('name-of-the-emitted-event', dataToPass)
      this.$emit("add:employee", this.employee);
      //   Adding a reference
      this.$refs.first.focus();
      this.employee = {
        name: " ",
        email: " "
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === " " || undefined || null;
    },
    invalidEmail() {
      return this.employee.email === " " || undefined || null;
    }
  }
};
</script>

<style scoped>
label {
  text-align: left;
}
form {
  margin-bottom: 2rem;
}
[class*="-message"] {
  font-weight: 500;
}
.error-message {
  color: #d33c40;
}
.success-message {
  color: #32a95d;
}
</style>
