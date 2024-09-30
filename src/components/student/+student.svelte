<script>
    import { onMount } from "svelte";
    import AddStudent from "./AddStudent.svelte"; // Ensure this path is correct

    let students = [];
    let loading = true;
    let error = null;

    // Function to fetch students from the backend API
    const fetchStudents = async () => {
        loading = true;
        error = null; // Reset error state

        try {
            const response = await fetch("http://localhost:8080/api/students"); // Adjust the URL as needed
            if (!response.ok) {
                throw new Error(`Error fetching students: ${response.status} ${response.statusText}`);
            }
            students = await response.json();
        } catch (err) {
            error = err.message; // Capture error message
        } finally {
            loading = false; // Always reset loading state
        }
    };

    // Fetch students when the component mounts
    onMount(() => {
        fetchStudents();
    });

    // Handle the event when a student is added
    const handleAddStudent = async (event) => {
        const newStudent = event.detail; // Get new student data from event

        try {
            const response = await fetch("http://localhost:8080/api/students", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(newStudent), // Convert new student data to JSON
            });

            if (!response.ok) {
                throw new Error(`Failed to add student: ${response.status} ${response.statusText}`);
            }

            // Fetch updated student list after adding
            await fetchStudents();
        } catch (err) {
            alert(err.message); // Alert on error
        }
    };
</script>

<h1 class="title">Student List</h1>

<AddStudent on:add={handleAddStudent} /> <!-- AddStudent component for adding students -->

{#if loading}
    <p class="loading">Loading...</p>
{:else if error}
    <p class="error">Error: {error}</p>
{:else if students.length === 0}
    <p class="empty">No students found.</p>
{:else}
    <ul class="student-list">
        {#each students as student}
            <li class="student-item">
                {student.firstName} {student.lastName} <!-- Display each student's first and last names -->
            </li>
        {/each}
    </ul>
{/if}

<style>
    .title {
        font-size: 2rem;
        margin-bottom: 20px;
        color: #333;
    }

    .loading, .error, .empty {
        color: #666;
        font-size: 1.2rem;
    }

    .student-list {
        list-style-type: none;
        padding: 0;
    }

    .student-item {
        padding: 10px;
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 1.1rem;
    }

    .student-item:hover {
        background-color: #f1f1f1; /* Highlight on hover */
    }
</style>
