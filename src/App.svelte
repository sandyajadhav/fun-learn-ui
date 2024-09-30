<script>
  import { Router, Route, Link } from "svelte-routing";
  import Login from "./components/login/+login.svelte"; // Import the login component
  import StudentList from "./components/student/+student.svelte"; // Import your student list component

  let isAuthenticated = false; // Track authentication status

  // Handle successful login
  const handleLoginSuccess = () => {
    isAuthenticated = true; // Update authentication status
  };
</script>

<Router>
  <Route path="/login" component={Login} on:login={handleLoginSuccess} />
  <Route path="/student" component={StudentList} />
</Router>

<style>
  nav {
    padding: 1rem;
    background: #f4f4f4;
  }
  nav a {
    margin-right: 1rem;
    text-decoration: none;
  }
</style>
