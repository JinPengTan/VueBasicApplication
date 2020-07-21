<template>
  <div id="app">
    <div class="small-container">
      <h1>Employees</h1>
      <employee-form
              @add:employee="addEmployee"/>
      <employee-table
              :employees="employees"
              :success="success"
              @delete:employee="deleteEmployee"
              @edit:employee="editEmployee"/>
    </div>

  </div>
</template>

<script>
import EmployeeTable from "./components/EmployeeTable";
import EmployeeForm from "./components/EmployeeForm";

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm,

  }, data() {
    return {
      employees: [],
      success: false,
    }
  },
  mounted() {
    this.getEmployees()
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users`)
        const data = await response.json();
        this.employees = data;
      } catch (e) {
        console.error(e);
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users`, {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {'Content-type': 'application/json; charset=UTF-8'},
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (e) {
        console.error(e);
      }
    },
    async editEmployee(id,employee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(employee),
          headers: {'Content-type': 'applicationjson; charset=UTF-8'},
        })
        const data = await response.json();
        this.employees = this.employees.map(employee => employee.id === id ? data : employee)
      } catch (e) {
        console.error(e)
      }
    },
    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE"
        });
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 1200px;
  }
</style>
