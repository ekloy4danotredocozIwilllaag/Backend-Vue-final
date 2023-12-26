<template>
    <div class="everything">
      <div class="table-rents">
        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>User ID</th>
              <th>Course</th>
              <th>Year</th>
              <th>Delete</th> 
            </tr>
          </thead>
          <tbody>
            <tr v-for="student in students" :key="student.id">
              <td>{{ student.id }}</td>
              <td>{{ student.user_id }}</td>
              <td>{{ student.course }}</td>
              <td>{{ student.year }}</td>
              <td>
                <button @click="deleteStudent(student.id)" class="btn btn-danger">Delete</button>
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
  
  const students = ref(null);
  
  onMounted(() => {
    // Fetch rents with customer data
    fetch('http://localhost:8000/api/students?include=users')
      .then(response => response.json())
      .then(data => (students.value = data))
      .catch(error => console.error('Error fetching data:', error));
  });
  
  const deleteStudent = async (jobId) => {
    try {
      const response = await fetch(`http://localhost:8000/api/students/${studentId}`, {
        method: 'DELETE',
      });
  
      if (response.ok) {
        // Filter out the deleted rent
        students.value = students.value.filter(student => student.id !== studentId);
  
        // Display an alert
        window.alert('student deleted successfully!');
      } else {
        console.error('Failed to delete student:', response.statusText);
      }
    } catch (error) {
      console.error('Error deleting student:', error);
    }
  };
  </script>