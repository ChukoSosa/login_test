<template>
    <div class="container">
        <div class="form-container">
            <form @submit.prevent="handleLogin" class="form">
                <img src="https://unsplash.it/200/300?image=12" alt="login image" />
                <!-- form header -->
                <h5 class="form-title">
                    <template v-if="username == ''">
                        Login
                    </template>
                    <template v-else>
                        {{ username }}
                    </template>
                </h5>

                <!-- inputs group -->
                <div class="form-group">
                    <label for="username">Username:</label>
                    <input 
                        type="text" 
                        id="username" 
                        v-model="username" 
                        required 
                        class="form-control" 
                        :disabled="loginSuccess"
                    />
                </div>
                <div class="form-group">
                    <label for="password">Password:</label>
                    <input 
                        type="password" 
                        id="password" 
                        v-model="password" 
                        required 
                        class="form-control" 
                        :disabled="loginSuccess"
                    />
                </div>

                <!-- loading spinner after submit -->
                <div v-if="loginSuccess" class="spinner-container">
                    <div class="spinner"></div>
                </div>
                
                <!-- submit button -->
                <template v-else>
                    <button type="submit" class="btn btn-primary">Login</button>
                </template>

            </form>

            <!-- status message -->
            <div v-if="loginMessage" class="alert" :class="{ 'alert-success': loginSuccess, 'alert-danger': !loginSuccess }">
                {{ loginMessage }}
            </div>
        </div>

    </div>
</template>

<script lang="ts">
import axios from 'axios'

export default {
    data() {
        return {
            username: '',
            password: '',
            loginMessage: '',
            loginSuccess: false,
            loading: false,
        }
    },
    methods: {
        async handleLogin() {
            this.loading = true;
            this.loginMessage = '';
            
            try {
                const data = {
                    username: this.username,
                    password: this.password
                };

                const res = await axios.post('http://localhost:3000/login', data);
                if (res.data.success) {
                    localStorage.setItem('token', res.data.token);
                    this.loginSuccess = true;

                    setTimeout(() => {
                        this.$router.push({ path: '/home' });
                    }, 3000)

                } else {
                    this.loginMessage = 'Login Error';
                }
            } catch (error) {
                this.loginMessage = 'Login Error';
            } finally {
                this.loading = false;
            }
        }
    }
}
</script>

<style scoped lang="scss">
.container {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}
.form-container {
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 20px;
    background-color: #fff;
    width: 400px;
}
img {
    border-radius: 50%;
    width: 120px;
    height: 120px;
    margin: 20px auto;
    display: block;
}
.form-title {
    text-align: center;
    margin-bottom: 30px;
}
.form-group {
    text-align: left;
    margin-bottom: 20px;
}
.btn-primary {
    background-color: #007bff;
    color: #fff;
    border-color: #007bff;
    width: 100%;
    padding: 10px;
    border-radius: 5px;
    font-size: 16px;
}
.alert {
    margin-top: 20px;
    padding: 20px;
    border-radius: 5px;
    font-size: 16px;
    text-align: center;
}
.alert-success {
    background-color: #d4edda;
    color: #155724;
    border-color: #c3e6cb;
}
.alert-danger {
    background-color: #f8d7da;
    color: #721c24;
    border-color: #f5c6cb;
}
.spinner-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
.spinner {
    border: 5px solid #f3f3f3;
    border-top: 5px solid #3498db;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
</style>
