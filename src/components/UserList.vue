<template>
  <div class="user-list-scroll-container">
    <h2>List of Users:</h2>
    <div class="user-list-container">
      <table>
        <thead>
          <tr>
            <th>Username</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.username }}</td>
            <td>
              <router-link class="link" :to="{ name: 'userDetails', params: { id: user.id } }">
                View Details
              </router-link>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      users: [],
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      const accessToken = localStorage.getItem('jwt');
      axios
        .get('http://localhost:9090/api/user', {
          headers: {
            Authorization: 'Bearer ' + accessToken,
          },
        })
        .then(response => {
          this.users = response.data;
        })
        .catch(error => {
          console.error('Error fetching users', error);
        });
    },
  },
};
</script>

<style scoped>
.user-list-scroll-container {
  font-family: 'Inter', sans-serif;
  width: 700px;
  margin: 0 auto;
  position: relative;
}

.user-list-container {
  margin-top: 20px;
  max-width: 700px;
  overflow-y: auto;
  max-height: 700px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

thead {
  background-color: darkgrey;
  color: #fff;
}

th,
td {
  border: 1px solid #ddd;
  padding: 10px; /* Adjusted padding for better spacing */
  text-align: left;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

.link {
  text-decoration: none;
  color: #0077ff;
  cursor: pointer;
}

.link:hover {
  text-decoration: underline;
}
</style>
