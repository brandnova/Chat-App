<template>
    <div class="bg-white w-full h-screen border flex flex-col items-center justify-center">
      <div class="my-auto">
        <div class="flex">
            <button 
            @click="activeTab = 'login'" 
            :class="['w-1/2 py-4 text-center font-semibold', activeTab === 'login' ? 'bg-white text-gray-800' : 'bg-gray-100 text-gray-600']"
            class="hover:bg-white focus:outline-none transition duration-300 ease-in-out">
            Login
            </button>
            <button 
            @click="activeTab = 'signup'" 
            :class="['w-1/2 py-4 text-center font-semibold', activeTab === 'signup' ? 'bg-white text-gray-800' : 'bg-gray-100 text-gray-600']"
            class="hover:bg-white focus:outline-none transition duration-300 ease-in-out">
            Sign Up
            </button>
        </div>
    
        <!-- Login Form -->
        <div v-show="activeTab === 'login'" class="p-8 animate-fadeIn">
            <h2 class="text-2xl font-bold text-center text-gray-800 mb-8">Welcome Back!</h2>
                <form @submit.prevent="login" class="space-y-3">
                <div class="mb-6 relative">
                    <i class="fas fa-user absolute text-gray-400 top-3 left-4"></i>
                    <input type="text" v-model="username" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(username)" placeholder="Username" required />
                </div>
                <div class="mb-6 relative">
                    <i class="fas fa-lock absolute text-gray-400 top-3 left-4"></i>
                    <input type="password" v-model="password" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(password)" placeholder="Password" required />
                </div>
                <button type="submit" class="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition duration-300 ease-in-out">
                    Login
                </button>
                </form>
            <p class="text-center mt-4 text-sm text-gray-600">Forgot your password? <a href="#" class="text-blue-600 hover:underline">Reset it here</a></p>
        </div>
    
        <!-- Signup Form -->
        <div v-show="activeTab === 'signup'" class="p-8 animate-fadeIn">
            <h2 class="text-2xl font-bold text-center text-gray-800 mb-8">Create an Account</h2>
            <form @submit.prevent="signup">
                <div class="mb-4 relative">
                    <i class="fas fa-user absolute text-gray-400 top-3 left-4"></i>
                    <input type="text" v-model="username" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(username)" placeholder="Username" required />
                </div>
                <div class="mb-4 relative">
                    <i class="fas fa-envelope absolute text-gray-400 top-3 left-4"></i>
                    <input type="email" v-model="email" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(email)" placeholder="Email" required />
                </div>
                <div class="mb-4 relative">
                    <i class="fas fa-user-circle absolute text-gray-400 top-3 left-4"></i>
                    <input type="text" v-model="first_name" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(first_name)" placeholder="First Name" required />
                </div>
                <div class="mb-4 relative">
                    <i class="fas fa-user-circle absolute text-gray-400 top-3 left-4"></i>
                    <input type="text" v-model="last_name" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(last_name)" placeholder="Last Name" required />
                </div>
                <div class="mb-6 relative">
                    <i class="fas fa-lock absolute text-gray-400 top-3 left-4"></i>
                    <input type="password" v-model="password" class="w-full pl-12 pr-4 py-2 rounded-lg border" 
                    :class="inputClass(password)" placeholder="Password" required />
                </div>
                <button type="submit" class="w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700 transition duration-300 ease-in-out">
                    Sign Up
                </button>
            </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { loginRest, signupRest } from "./api";
  
  export default {
    data() {
      return {
        activeTab: "login", // For toggling between login and signup
        username: "",
        password: "",
        email: "",
        first_name: "",
        last_name: "",
      };
    },
    methods: {
      login() {
        loginRest(this.username, this.password)
          .then((response) => {
            this.$emit("onAuth", { ...response.data, secret: this.password });
          })
          .catch((error) => console.log("Login error", error));
      },
      signup() {
        signupRest(this.username, this.password, this.email, this.first_name, this.last_name)
          .then((response) => {
            this.$emit("onAuth", { ...response.data, secret: this.password });
          })
          .catch((error) => console.log("Sign up error", error));
      },
      // Input validation class handling
      inputClass(input) {
        return input.length > 0 ? "border-green-500" : "border-red-500";
      },
    },
  };
  </script>
  
  <style scoped>
  .animate-fadeIn {
    animation: fadeIn 0.5s ease-in-out;
  }
  @keyframes fadeIn {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }
  </style>
   