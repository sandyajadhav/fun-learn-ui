<script lang="ts">
    import { createEventDispatcher } from 'svelte';
    import { navigate } from 'svelte-routing';

    const dispatch = createEventDispatcher();
    let username: string = '';
    let password: string = '';
    let loginError: string = '';

    const handleLogin = async (event: Event) => {
        event.preventDefault();
        loginError = ''; // Reset the error message

        try {
            const response = await fetch('http://localhost:8080/login', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ username, password }),
            });

            if (!response.ok) {
                throw new Error('Invalid username or password');
            }

            const data = await response.json();
            // Store the token in session storage
            sessionStorage.setItem('authToken', data.token);

            // Handle successful login, e.g., redirect or store user info
            navigate('/student');
            // You might want to redirect or navigate to a different page here
        } catch (error) {
            loginError = error.message;
        }
    };
</script>

<style>
    body {
        margin: 0;
        font-family: 'Arial', sans-serif;
    }

    .login-container {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background: url('https://source.unsplash.com/random/1920x1080') no-repeat center center fixed;
        background-size: cover;
    }

    .login-form {
        background: rgba(255, 255, 255, 0.9);
        padding: 40px;
        border-radius: 15px;
        box-shadow: 0 4px 40px rgba(0, 0, 0, 0.2);
        width: 400px;
        text-align: center;
    }

    .login-form h2 {
        margin-bottom: 30px;
        color: #333;
        font-weight: 600;
    }

    .login-form label {
        display: block;
        margin-top: 15px;
        text-align: left;
        color: #555;
        font-weight: 500;
    }

    .login-form input {
        width: 100%;
        padding: 12px;
        margin-top: 5px;
        margin-bottom: 20px;
        border-radius: 10px;
        border: 1px solid #ddd;
        font-size: 16px;
        transition: border-color 0.3s;
        font-family: 'Arial', sans-serif;
    }

    .login-form input:focus {
        border-color: #6a11cb;
        outline: none;
    }

    .login-form button {
        width: 100%;
        padding: 12px;
        background-color: #6a11cb;
        color: white;
        border: none;
        border-radius: 10px;
        font-size: 16px;
        cursor: pointer;
        transition: background-color 0.3s;
        font-weight: bold;
    }

    .login-form button:hover {
        background-color: #2575fc;
    }

    .error {
        color: red;
        margin-top: 10px;
        font-weight: 500;
    }

    .forgot-password {
        margin-top: 15px;
        font-size: 14px;
        color: #6a11cb;
    }

    .forgot-password:hover {
        text-decoration: underline;
    }

    @media (max-width: 480px) {
        .login-form {
            width: 90%;
            padding: 30px;
        }
    }
</style>

<div class="login-container">
    <form class="login-form" on:submit={handleLogin}>
        <h2>Login</h2>

        <label for="username">Username:</label>
        <input
                type="text"
                id="username"
                placeholder="Enter username"
                bind:value={username}
                required
        />

        <label for="password">Password:</label>
        <input
                type="password"
                id="password"
                placeholder="Enter password"
                bind:value={password}
                required
        />

        <button type="submit">Login</button>

        {#if loginError}
            <div class="error">{loginError}</div>
        {/if}

        <a href="#" class="forgot-password">Forgot Password?</a>
    </form>
</div>
