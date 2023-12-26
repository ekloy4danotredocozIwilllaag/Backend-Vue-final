<template>
    <div class="everything">
      <div class="table-rents">
        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>User ID</th>
              <th>Status</th>
              <th>occupation</th>
              <th>Delete</th> 
            </tr>
          </thead>
          <tbody>
            <tr v-for="job in jobs" :key="job.id">
              <td>{{ job.id }}</td>
              <td>{{ job.user_id }}</td>
              <td>{{ job.status }}</td>
              <td>{{ job.occupation }}</td>
              <td>
                <button @click="deleteJob(job.id)" class="btn btn-danger">Delete</button>
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
  border-bottom: 1px solid #ff4081; /* Pink border */
  padding-bottom: 10px;
  display: flex;
  
  }
  
  .table-rents {
  margin-top: 20px;
  }
  
  table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  }
  
  th, td {
  border: 1px solid #ff4081; /* Pink border */
  padding: 8px;
  text-align: left;
  }
  
  th {
  background-color: #ff4081; /* Pink background for header */
  color: white; /* White text for header */
  }
  
  .rentT {
  color: white; /* White color for h1 */
  }
  .plus{
  padding: 5px;
  color: #ff4081;
  background-color: white;
  border-radius: 5px;
  font-size: 15px;
  text-decoration: none;
  }
  button{
  height: 30px;
  width: 30px;
  }
  </style>
  
  <script setup>
  import { onMounted, ref } from 'vue';
  
  const jobs = ref(null);
  
  onMounted(() => {
    // Fetch rents with customer data
    fetch('http://localhost:8000/api/jobs?include=users')
      .then(response => response.json())
      .then(data => (jobs.value = data))
      .catch(error => console.error('Error fetching data:', error));
  });
  
  const deleteJob = async (jobId) => {
    try {
      const response = await fetch(`http://localhost:8000/api/jobs/${jobId}`, {
        method: 'DELETE',
      });
  
      if (response.ok) {
        // Filter out the deleted rent
        jobs.value = jobs.value.filter(job => job.id !== jobId);
  
        // Display an alert
        window.alert('job deleted successfully!');
      } else {
        console.error('Failed to delete job:', response.statusText);
      }
    } catch (error) {
      console.error('Error deleting job:', error);
    }
  };
  </script>