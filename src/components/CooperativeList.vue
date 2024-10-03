<template>
  <div>
    <h2>All Cooperatives:</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>VAT</th>
          <th>Details</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cooperative in cooperatives" :key="cooperative.id">
          <td>{{ cooperative.name }}</td>
          <td>{{ cooperative.vat }}</td>
          <td>
            <router-link :to="{ name: 'CooperativeDetails', params: { id: cooperative.id } }">
              View Details
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cooperatives: [],
    };
  },

  created() {
    this.fetchCooperatives();
  },

  methods: {
    fetchCooperatives() {
      const accessToken = localStorage.getItem('jwt');

      axios.get('http://localhost:9090/api/cooperative', {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          this.cooperatives = response.data;
        })
        .catch(error => {
          console.error('Error fetching cooperatives', error);
        });
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

thead {
  background-color: #f2f2f2;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
}

a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
