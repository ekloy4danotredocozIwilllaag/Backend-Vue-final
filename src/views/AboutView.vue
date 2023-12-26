<template>
  <div class="everything">
    <div class="perin">
      
    </div>
    <div class="table-customers">
      <table class="table table-bordered table-striped">
        <thead>
          <tr>
            <th>ID</th>
            <th>User Name</th>
            <th>Email</th>
            <th>Full Name</th>
            <th>Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.full_name }}</td>
            <td>
              <button @click="deleteUser(user.id)" class="btn btn-danger">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style>
.everything {
  margin: 3.5rem 4rem;
}

.perin h1 {
  font-size: 45px;
  margin-left: 2px;
  font-weight: 1000;
  letter-spacing: -2px;
  font-family: 'Cavilant';
}

.perin {
  border-bottom: 1px solid #4b494a; /* Pink border */
  padding-bottom: 10px;
  display: flex;
}

.table-customers {
  margin-top: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  border-radius: 10px;
}

th, td {
  border: 1px solid #faf4f4; /* Pink border */
  padding: 8px;
  text-align: left;
}

th {
  background-color: #50373f; /* Pink background for header */
  color: white; /* White text for header */
}

.cust {
  color: white; /* White color for h1 */
}

.plus {
  padding: 5px;
  color: #ff4081;
  background-color: white;
  border-radius: 5px;
  font-size: 15px;
  text-decoration: none;
}

.btn {
  background-color: #dc3545; /* Bootstrap red color */
  color: #ffffff;
  border-radius: 5px;
  padding: 5px 10px;
}
</style>

<script setup>
import { onMounted, ref } from 'vue';

const users = ref(null);

onMounted(() => {
  fetch('http://localhost:8000/api/users')
    .then(response => response.json().then(data => (users.value = data)));
});

const deleteUser = async (userId) => {
  try {
    const response = await fetch(`http://localhost:8000/api/users/${userId}`, {
      method: 'DELETE',
    });

    if (response.ok) {
      users.value = users.value.filter((user) => user.id !== userId);
      alert('User deleted successfully!');
    } else {
      console.error('Failed to delete User:', response.statusText);
    }
  } catch (error) {
    console.error('Error deleting user:', error);
  }
};
</script>

