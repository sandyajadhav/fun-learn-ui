<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();
    let firstName = ""; // Variable to store the student's first name
    let lastName = "";  // Variable to store the student's last name

    const addStudent = () => {
        if (firstName.trim() && lastName.trim()) {
            dispatch("add", { firstName, lastName }); // Emit the add event with first and last names
            firstName = ""; // Reset the first name input field
            lastName = "";  // Reset the last name input field
        } else {
            alert("Please enter valid first and last names."); // Alert if inputs are invalid
        }
    };
</script>

<div class="add-student-form">
    <input type="text" bind:value={firstName} placeholder="First Name" class="input" />
    <input type="text" bind:value={lastName} placeholder="Last Name" class="input" />
    <button on:click={addStudent} class="button">Add Student</button>
</div>

<style>
    .add-student-form {
        display: flex;
        gap: 10px; /* Space between inputs */
        margin-bottom: 20px;
    }

    .input {
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        flex: 1; /* Make inputs take equal space */
    }

    .button {
        padding: 10px 20px;
        background-color: #007bff; /* Bootstrap primary color */
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .button:hover {
        background-color: #0056b3; /* Darken button on hover */
    }
</style>
