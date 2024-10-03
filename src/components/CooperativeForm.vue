<template>
  <div>
    <h2>Fill in the Form</h2>
    <form @submit.prevent="saveCooperative">
      <div>
        <label for="name">Name:</label>
        <input v-model="cooperative.name" type="text" id="name" required>
      </div>

      <div>
        <label for="vat">VAT:</label>
        <input v-model="cooperative.vat" type="text" id="vat" required>
      </div>

      <div>
        <label for="product_name">Product Name:</label>
        <input v-model="cooperative.product_name" type="text" id="product_name" required>
      </div>

      <div>
        <label for="product_category">Product Category:</label>
        <select v-model="cooperative.product_category" id="product_category" required>
          <option value="vegetables">Vegetables</option>
          <option value="fruits">Fruits</option>
          <option value="meat">Meat</option>
          <option value="milk">Milk</option>
          <option value="chocolate">Chocolate</option>
          <option value="honey">Honey</option>
          <option value="bread">Bread</option>
          <option value="olive">Olive</option>
        </select>
      </div>

      <div>
        <label for="cultivation_city">City:</label>
        <input v-model="cooperative.cultivation_city" type="text" id="cultivation_city" required>
      </div>

      <div>
        <label for="cultivation_address">Address:</label>
        <input v-model="cooperative.cultivation_address" type="text" id="cultivation_address" required>
      </div>

      <div>
        <label for="cultivation_street_number">Street Number:</label>
        <input v-model="cooperative.cultivation_street_number" type="text" id="cultivation_street_number" required>
      </div>

      <div>
        <label for="members">Members:</label>
        <select v-model="cooperative.members" id="members" required>
          <option value="1-5">1-5</option>
          <option value="5-10">5-10</option>
          <option value="10-20">10-20</option>
          <option value="20-40">20-40</option>
          <option value="40+">40+</option>
        </select>
      </div>

      <button type="submit">Save</button>

      <div v-if="submissionCompleted">Submission Completed</div>
    </form>
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
        cultivation_city: '',
        cultivation_address: '',
        cultivation_street_number: '',
        members: ''
      },
      submissionCompleted: false,
    };
  },

  methods: {
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
        this.submissionCompleted = true;
      })
      .catch(error => {
        console.error('Error saving cooperative', error);
      });
    },
  },
};
</script>

<style scoped>
/* Minimal styles for simplicity */
div {
  margin: 10px 0;
}

label {
  display: block;
}

input,
select {
  width: 100%;
  padding: 5px;
  margin: 5px 0;
}

button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
