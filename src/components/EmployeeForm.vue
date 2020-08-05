<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label>Employee name</label>
            <input
                type="text"
                ref="first"
                :class="{ 'has-error': submitting && invalidName }"
                v-model="employee.name"
                @focus="clearStatus"
                @keypress="clearStatus"
            />
            <label>Employee Email</label>
            <input
                type="text"
                :class="{ 'has-error': submitting && invalidEmail }"
                v-model="employee.email"
                @focus="clearStatus"
            />
            <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
            <p v-if="success" class="success-message">✅ Employee successfully added</p>
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
                email: "",
            },
        };
    },
    methods: {
        handleSubmit() {
            // Change submitting state to true
            this.submitting = true;

            // Clear success, error statuses
            this.clearStatus();

            // Check if name, email are entered incorrectly
            if (this.invalidName || this.invalidEmail) {
                this.error = true;
                return;
            }

            // Emit new employee to parent
            this.$emit("add:employee", this.employee);

            // Return focus to first input
            this.$refs.first.focus();

            // Reset employee info inputs
            this.employee = {
                name: "",
                email: "",
            };

            // Set result status
            this.error = false;
            this.success = true;
            this.submitting = false;
        },

        clearStatus() {
            this.error = false;
            this.success = false;
        },
    },
    computed: {
        invalidName() {
            return this.employee.name === "";
        },

        invalidEmail() {
            return this.employee.email === "";
        },
    },
};
</script>

<style scoped>
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