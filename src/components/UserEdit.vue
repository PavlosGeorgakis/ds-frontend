<template>
  <div>
    <h1>Update User</h1>
    <form @submit.prevent="updateUser">
      <label for="username">Username:</label>
      <input v-model="userDetails.username" type="text" id="username" required>

      <label for="email">Email:</label>
      <input v-model="userDetails.email" type="email" id="email" required>

      <label for="password">Password:</label>
      <input v-model="userDetails.password" type="password" id="password" required>

      <button type="submit">Update</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userDetails: {
        username: '',
        email: '',
        password: '',
      },
    };
  },
  created() {
    const userId = this.$route.params.id;
    this.fetchUserDetails(userId);
  },
  methods: {
    fetchUserDetails(userId) {
      const accessToken = localStorage.getItem('jwt');
      axios.get(`http://localhost:9090/api/user/admin/${userId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          // Pre-fill the form fields with existing data
          this.userDetails = response.data;
        })
        .catch(error => {
          console.error('Error fetching user details', error);
        });
    },
    updateUser() {
      const userId = this.$route.params.id;
      const accessToken = localStorage.getItem('jwt');

      axios.post(`http://localhost:9090/api/user/new`, this.userDetails, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
          'Content-Type': 'application/json',
        },
      })
        .then(response => {
          console.log('User updated successfully', response.data);
          // Redirect to the user details page or perform other actions
          this.$router.push(`/users/${userId}`);
        })
        .catch(error => {
          console.error('Error updating user', error);
        });
    },
  },
};
</script>

<style scoped>
h1 {
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 0 auto;
}

label {
  margin-bottom: 5px;
  font-weight: bold;
}

input {
  margin-bottom: 16px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
}

button:hover {
  background-color: #0056b3;
}
</style>
