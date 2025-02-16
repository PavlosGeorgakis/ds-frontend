<template>
  <div class="cooperative-details">
    <table v-if="cooperativeDetails.name">
      <tbody>
        <tr>
          <td><strong>Name:</strong></td>
          <td>{{ cooperativeDetails.name }}</td>
        </tr>
        <tr v-if="cooperativeDetails.status">
          <td><strong>Status:</strong></td>
          <td>{{ cooperativeDetails.status }}</td>
        </tr>
        <tr v-if="cooperativeDetails.notes">
          <td><strong>Notes:</strong></td>
          <td>{{ cooperativeDetails.notes }}</td>
        </tr>
        <tr>
          <td><strong>User ID:</strong></td>
          <td>{{ userDetails.id }}</td>
        </tr>
      </tbody>
    </table>

    <p v-if="approvedNotes"><strong>Notes:</strong> {{ approvedNotes }}</p>

    <div v-if="cooperatives.length">
      <h2>Cooperatives:</h2>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Status</th>
            <th>Notes</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="coop in cooperatives" :key="coop.id">
            <td>{{ coop.name }}</td>
            <td>{{ coop.status }}</td>
            <td>{{ coop.notes }}</td>
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
      cooperativeDetails: {},
      userDetails: {},
      cooperatives: [],
      approvedStatus: '',
      approvedNotes: '',
    };
  },

  async created() {
    await this.fetchUserDetails();
    await this.fetchCooperatives();
  },

  methods: {
    async fetchUserDetails() {
      const storedUser = localStorage.getItem('user');
      this.userDetails = storedUser ? JSON.parse(storedUser) : {};
    },

    async fetchCooperatives() {
      const accessToken = localStorage.getItem('jwt');

      if (!this.userDetails.id) {
        console.error('User ID is undefined.');
        return;
      }

      try {
        const response = await axios.get(`http://localhost:9090/api/cooperative/user/${this.userDetails.id}/cooperatives/details`, {
          headers: {
            'Authorization': 'Bearer ' + accessToken,
          },
        });
        this.cooperatives = response.data;
      } catch (error) {
        console.error('Error fetching cooperatives', error);
      }
    },
  },

  beforeRouteEnter(to, from, next) {
    next(vm => {
      vm.approvedStatus = to.query.status || '';
      vm.approvedNotes = to.query.notes || '';
    });
  },
};
</script>

<style scoped>
.cooperative-details {
  font-family: 'Inter', sans-serif;
  width: 600px;
  max-width: 600px;
  position: absolute;
  top: 200px;
  left: 50%;
  transform: translate(-50%, 0);
  border-radius: 50px;
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
  padding: 8px;
  text-align: left;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

strong {
  font-weight: bold;
}
</style>
