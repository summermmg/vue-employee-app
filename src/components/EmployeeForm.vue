<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label>Employee Name</label>
            <!-- mark error by adding class and css of the class -->
            <input 
                v-model="employee.name" 
                type="text" 
                v-bind:class="{'has-error': submitting && invalidName && error}"
                v-on:focus="statusReset"                
            />

            <label>Employee Email</label>
            <input 
                v-model="employee.email"
                type="email" 
                v-bind:class="{'has-error': submitting && invalidEmail && error}" 
                v-on:focus="statusReset"
            />

            <!-- conditionally render p tag -->
            <p v-if="error && submitting" class="error-message">
                Please fill out required fields.    
            </p> 

            <p v-if="success" class="success-message">
                Employee successfully added 
            </p>   
            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name: "employee-form",
    // Set up initial state
    data() {
        return {
            employee: {
                name: '',
                email: '',
            },
            // to check if the form is currently submitted
            submitting: false,
            error: false,
            success: false,
        }
    },
    computed: {
        // basic form validation
        invalidName: function() {
            return this.employee.name == '';
        },
        invalidEmail: function() {
            return this.employee.email == '';
        }
    },
    methods: {
        handleSubmit() {
            // console.log('testing handleSubmit')
            this.submitting = true;
            this.statusReset();

            if (this.invalidName || this.invalidEmail) {
                // alert error message
                this.error = true;
                console.log("error")
                return
            } 
            // Emit add-employee event to parent component 
            this.$emit('add-employee', this.employee)
            this.error = false;
            this.success = true;
            this.submitting = false;
            this.employee.name="";
            this.employee.email="";
        },
        statusReset: function() {
            this.error = false;
            this.success = false;
        } 
    }
}
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }
     /* class属性中包含-message的每个元素 */
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