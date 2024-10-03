<template>
  <div>
    <p>Name: {{ cooperativeDetails.name }}</p>
    <p>VAT: {{ cooperativeDetails.vat }}</p>
    <p>Product Name: {{ cooperativeDetails.product_name }}</p>
    <p>Status: {{ cooperativeDetails.status }}</p>
    <p>Notes: {{ cooperativeDetails.notes }}</p>

    <button class="deleteB" @click="deleteCooperative">Delete</button>
    <button class="approveB" @click="approveCooperative(cooperativeDetails.id)">Approve</button>
    <button class="rejectB" @click="rejectCooperative(cooperativeDetails.id)">Reject</button>

    <p>Your User ID as Moderator is: {{ userDetails.id }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cooperativeDetails: {},
      userDetails: {},
    };
  },

  created() {
    this.fetchCooperativeDetails();
    this.fetchUserDetails();
  },

  methods: {
    fetchUserDetails() {
      const storedUser = localStorage.getItem('user');
      if (storedUser) {
        this.userDetails = JSON.parse(storedUser);
      } else {
        return null;
      }
    },

    fetchCooperativeDetails() {
      const accessToken = localStorage.getItem('jwt');
      const cooperativeId = this.$route.params.id;

      axios.get(`http://localhost:9090/api/cooperative/${cooperativeId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          this.cooperativeDetails = response.data;
        })
        .catch(error => {
          console.error('Error fetching cooperative details', error);
        });
    },

    approveCooperative(cooperativeId) {
      const notes = prompt('Enter notes for approval:');
      if (notes !== null) {
        const accessToken = localStorage.getItem('jwt');
        const userId = this.userDetails.id;

        axios.post(`http://localhost:9090/api/user/employee/${userId}/${cooperativeId}/check/submit`, {
          notes: notes,
        }, {
          headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json',
          },
        })
          .then(response => {
            console.log('Cooperative approved successfully', response.data);
            this.fetchCooperativeDetails();
          })
          .catch(error => {
            console.error('Error approving cooperative', error);
          });
      }
    },

    rejectCooperative(cooperativeId) {
      const notes = prompt('Enter notes for rejection:');
      if (notes !== null) {
        const accessToken = localStorage.getItem('jwt');
        const userId = this.userDetails.id;

        axios.post(`http://localhost:9090/api/user/employee/${userId}/${cooperativeId}/check/submit-reject`, {
          notes: notes,
        }, {
          headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json',
          },
        })
          .then(response => {
            console.log('Cooperative rejected successfully', response.data);
            this.fetchCooperativeDetails();
          })
          .catch(error => {
            console.error('Error rejecting cooperative', error);
          });
      }
    },

    deleteCooperative() {
      const accessToken = localStorage.getItem('jwt');
      const cooperativeId = this.$route.params.id;

      axios.delete(`http://localhost:9090/api/cooperative/${cooperativeId}`, {
        headers: {
          'Authorization': 'Bearer ' + accessToken,
        },
      })
        .then(response => {
          console.log('Cooperative deleted successfully', response.data);
          this.$router.push('/cooperatives');
        })
        .catch(error => {
          console.error('Error deleting cooperative', error);
        });
    },
  },
};
</script>

<style scoped>
div {
  max-width: 700px;
  margin: 50px auto;
  padding: 50px;
  border-radius: 5px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
  background-color: #ffdddd;
  font-family: 'Comic Sans MS', cursive;
  font-size: 18px;
  color: #333;
}

button {
  padding: 5px;
  margin-right: 5px;
  font-size: 1rem;
  color: #000;
  background-color: #ffcc00;
  border: 2px solid #333;
  border-radius: 0;
  cursor: pointer;
}

button:hover {
  background-color: #ffff66;
}
</style>
