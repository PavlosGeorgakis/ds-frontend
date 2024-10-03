<template>
  <div>
    <div class="form-container">
      <form @submit.prevent="saveCooperative" class="cooperative-form">
        <label for="name">Name:</label>
        <input v-model="cooperative.name" type="text" id="name" required>

        <label for="vat">VAT:</label>
        <input v-model="cooperative.vat" type="text" id="vat" required>

        <label for="product_name">Product Name:</label>
        <input v-model="cooperative.product_name" type="text" id="product_name" required>

        <label for="product_category">Product Category:</label>
        <input v-model="cooperative.product_category" type="text" id="product_category" required>

        <button type="submit">Save</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cooperative: {
        name: '',
        vat: '',
        product_name: '',
        product_category: '',
      },
    };
  },
  created() {
    const cooperativeId = this.$route.params.id;
    this.fetchCooperativeDetails(cooperativeId);
  },
  methods: {
    fetchCooperativeDetails(cooperativeId) {
      const accessToken = localStorage.getItem('jwt');
      axios.get(`http://localhost:9090/api/cooperative/${cooperativeId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          // Pre-fill form fields with existing data
          this.cooperative = response.data;
        })
        .catch(error => {
          console.error('Error fetching cooperative details', error);
        });
    },
    saveCooperative() {
      const accessToken = localStorage.getItem('jwt');

      axios.post('http://localhost:9090/api/cooperative/new', this.cooperative, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
          'Content-Type': 'application/json',
        },
      })
        .then(response => {
          console.log('Cooperative saved successfully', response.data);
          const cooperativeId = this.$route.params.id;
          this.$router.push(`/cooperatives/${cooperativeId}`);
        })
        .catch(error => {
          console.error('Error saving cooperative', error);
        });
    },
  },
};
</script>

<style scoped>
.form-container {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 0px; /* No border radius */
  box-shadow: none; /* No shadow */
  background-color: #fff0f0; /* Light red background */
  font-family: 'Comic Sans MS', cursive; /* Comic Sans font */
  font-size: 20px; /* Larger font */
  border: 2px solid #ff0000; /* Red border */
}

.cooperative-form {
  display: block; /* Block display */
}

label {
  margin-bottom: 10px; /* Reduced margin */
  font-weight: bold; /* Bold text */
}

input {
  padding: 5px;
  margin-bottom: 10px;
  border: 2px solid #ff0000; /* Red border */
  border-radius: 0px; /* No border radius */
  outline: none;
  font-size: 18px; /* Larger font size */
}

button {
  margin-top: 15px;
  padding: 10px;
  background-color: #ffcc00; /* Yellow button */
  color: #000; /* Black text */
  border: 2px solid #333; /* Dark border */
  border-radius: 0px; /* No border radius */
  cursor: pointer;
  font-weight: bold; /* Bold text */
  font-size: 20px; /* Larger font size */
}

button:hover {
  background-color: #ffff00; /* Brighter yellow on hover */
}
</style>
