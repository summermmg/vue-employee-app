<template>
    <div id="employee-table">
        <table>
            <thead>
                <tr>
                    <th>Employee Name</th>
                    <th>Employee Email</th>
                </tr>
            </thead>
            <tbody>
                 <!-- loop through the data and display the DOM nodes -->
                <tr v-for="employee in employees" v-bind:key="employee.id">
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name" />
                    </td>
                    <td v-else>{{ employee.name }}</td>

                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email" />
                    </td>
                    <td v-else>{{ employee.email }}</td>

                    <td v-if="editing === employee.id">
                        <!-- add employee to the list -->
                        <button v-on:click="editEmployee(employee)">Save</button>
                        <button class="muted-button" v-on:click="cancelEmployee(employee)">Cancel</button>
                    </td>
                    <td v-else>
                        <button
                            v-on:click="enterEditMode(employee)"
                        >Edit</button>
                        <button
                            v-on:click="deleteEmployeeHandler(employee.id)"
                        >Delete</button>
                    </td>
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
    // local state
    data() {
        return {
            editing: null,
            cachedEmployee: null
        }
    },
    methods: {
        deleteEmployeeHandler: function(id) {
            this.$emit('delete-employee',id)
        },
        enterEditMode: function(employee) {
            if (this.editing) return
            // Cache current employee info so that it won't be actually changed after we cancel edit 
            this.cachedEmployee =  Object.assign({} , employee) 
            this.editing = employee.id;
        },
        cancelEmployee: function(employee) {
            Object.assign(employee, this.cachedEmployee)
            this.editing = null;
        },
        editEmployee: function(employee) {
            // update input can't be empty
            if (employee.name==="" || employee.email === "") return;
            // emit event to parent component
            this.$emit("edit-employee", employee)
            this.editing = null;
        }
    }
}

</script>

<style scoped>
    button {
        margin: 0 0.5rem;
    } 
</style>