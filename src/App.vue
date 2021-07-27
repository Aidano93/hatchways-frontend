<template>
  <div class="student-list-container">
    <div class="student-list" v-for="student in students" :key="student.id">
      <img :src="student.pic" :alt="'portrait of ' + student.firstName + ' ' + student.lastName">
      <h3>{{`${student.firstName} ${student.lastName}`}}</h3>
      <ul>
        <li>Email: {{student.email}}</li>
        <li>Company: {{student.company}}</li>
        <li>Skill: {{student.skill}}</li>
        <li>Average: {{getAverage(student.grades)}}% </li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return{
      students: [],
    }
  },
  mounted(){
    fetch('https://api.hatchways.io/assessment/students')
    .then((res) => {
        if (!res.ok) throw new Error('Something went wrong')
        return res.json();
      })
    .then((data) => {
        this.students = data.students
      })
    .catch(function(error){
        console.log(error);
      });  
  },
  methods: {
    getAverage(arr) {
      let sum = 0;
      for( let i = 0; i < arr.length; i++ ){
      sum += parseInt( arr[i], 10 );
      }
      const avg = sum/arr.length;
      return avg
    }
    }
}
</script>

<style>

</style>
