<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <!-- Call addEmployee method when receiving add-employee event -->
    <employee-form v-on:add-employee="addEmployee" />
    <!-- Pass props data to EmployeeTable component -->
    <employee-table 
      v-bind:employees="employees"
      v-on:delete-employee="deleteEmployee"  
      v-on:edit-employee="editEmployee"
    />
  </div>
</template>

<script>
// import component
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'


export default {
  name: 'App',
  // register component
  components: {
    EmployeeTable,
    EmployeeForm
  },
  // like React state
  data() {
    return {
      employees: []
    }
  },
  mounted() {
    this.fetchEmployees();
  },
  methods: {
    async fetchEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const data = await response.json();
        this.employees = data
        console.log('fetched employees!')
      } catch(err) {
        console.error(err) 
      }
    },
    // 自动接收子组件传递的值
    async addEmployee(employee) {
      // // Generate a new employee with unique ID
      // const newId = 
      //   this.employees.length > 0
      //   ? this.employees.length + 1
      //   : 1;
      // const newEmployee = {...employee, id: newId}
      // // update employees state
      // this.employees = [...this.employees, newEmployee]

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: "POST",
          headers: {
            'Content-type': 'application/json'
          },
          body: JSON.stringify(employee)
        });
        const data = await response.json();
        // console.log(data)
        this.employees = [...this.employees,data]
      } catch(err) {
        console.error(err)
      }
    },
    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE"
        });   
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch(err) {
        console.log(err)
      }
    },
    async editEmployee(updateEmployee) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${updateEmployee.id}`, {
          method: "PUT",
          headers: {
            "Content-type": 'application/json'
          },
          body: JSON.stringify(updateEmployee)
        })
      } catch(err) {
        console.error(err)
      }
    }
  }
}
</script>

<style>
  .small-container {
    max-width: 800px;
    display: flex;
    flex-direction: column;
  }
  .small-container>:first-child {
    align-self: center;
  }

</style>
