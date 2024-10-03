<template>
  <div>
    <h1>User Details</h1>
    <p>Name: {{ userDetails.username }}</p>
    <p>Encrypted Password: {{ userDetails.password }}</p>
    <p>Email: {{ userDetails.email }}</p>
    
    <button @click="editUser">Edit</button>
    <button @click="deleteUser">Delete</button>

    <h2>System Roles & Authorities</h2>
    <p>Roles: ADMIN (Number: 1)</p>
    <p>Roles: USER (Number: 4)</p>
    <p>Roles: MODERATOR (Number: 5)</p>
    
    <button @click="addRolePrompt">Add Role</button>
    <button @click="deleteRolePrompt">Delete Role</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userDetails: {},
    };
  },

  created() {
    this.fetchUserDetails();
  },

  methods: {
    fetchUserDetails() {
      const accessToken = localStorage.getItem('jwt');
      const userId = this.$route.params.id;

      axios.get(`http://localhost:9090/api/user/admin/${userId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          this.userDetails = response.data;
        })
        .catch(error => {
          console.error('Error fetching user details', error);
        });
    },

    addRolePrompt() {
      const userId = this.$route.params.id;
      const roleNumber = prompt('Enter role number to add:');

      if (roleNumber !== null) {
        this.addRole(userId, roleNumber);
      }
    },

    deleteRolePrompt() {
      const userId = this.$route.params.id;
      const roleNumber = prompt('Enter role number to delete:');

      if (roleNumber !== null) {
        this.deleteRole(userId, roleNumber);
      }
    },

    addRole(userId, roleNumber) {
      const accessToken = localStorage.getItem('jwt');

      axios.post(`http://localhost:9090/api/user/admin/role/add/${userId}/${roleNumber}`, null, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          console.log('Role added successfully', response.data);
          this.fetchUserDetails();
        })
        .catch(error => {
          console.error('Error adding role to user', error);
        });
    },

    deleteRole(userId, roleNumber) {
      const accessToken = localStorage.getItem('jwt');

      axios.post(`http://localhost:9090/api/user/admin/role/delete/${userId}/${roleNumber}`, null, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          console.log('Role deleted successfully', response.data);
          this.fetchUserDetails();
        })
        .catch(error => {
          console.error('Error deleting role from user', error);
        });
    },

    editUser() {
      const userId = this.$route.params.id;
      this.$router.push(`/users/${userId}/edit`);
    },

    deleteUser() {
      const accessToken = localStorage.getItem('jwt');
      const userId = this.$route.params.id;

      axios.delete(`http://localhost:9090/api/user/admin/${userId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          console.log('User deleted successfully', response.data);
          this.$router.push('/users');
        })
        .catch(error => {
          console.error('Error deleting user', error);
        });
    },
  },
};
</script>

<style scoped>
h1 {
  margin-bottom: 10px;
}

button {
  margin-right: 10px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}
</style>
