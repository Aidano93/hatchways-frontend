<template>
  <div class="student-list-container">
    <div class="student-list" v-for="student in students" :key="student.id">
      <div class="student-list-image">
        <img :src="student.pic" :alt="'portrait of ' + student.firstName + ' ' + student.lastName">
      </div>
      <div class="student-list-info">
        <h3>{{`${student.firstName} ${student.lastName}`}}</h3>
        <ul>
          <li>Email: {{student.email}}</li>
          <li>Company: {{student.company}}</li>
          <li>Skill: {{student.skill}}</li>
          <li>Average: {{getAverage(student.grades)}}% </li>
        </ul>
      </div>
      
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
        console.log(data)
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

<style lang="scss">

  .student-list-container {
    margin: auto;
    margin-top: 5rem;
    margin-bottom: 5rem;
    width: 60vw;
    height: 83vh;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    /* border: 1px solid gray; */
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 10px;

    &:hover {
      overflow-y: auto;
      

      &::-webkit-scrollbar {
        width: 0.5rem;
      }
      &::-webkit-scrollbar-track {
        background: transparent;
      }
      &::-webkit-scrollbar-thumb {
        background: #aaa3aa;
        border-radius: 10px;
      }   
    }

    .student-list {
      display: flex;
      padding: 1rem;
      flex-direction: row;
      align-items: center;
      border-bottom: 2px solid rgb(241, 241, 241);

      .student-list-image {
        display: flex;
        justify-content: center;

        img {
          width: 7rem;
          border-radius: 100%;
          border: 1px solid rgb(207, 206, 206);
        }
      }

      .student-list-info {
        margin-left: 2rem;

        h3 {
          text-transform: uppercase;
          font-size: 2.3rem;
          margin-bottom: 0.7rem;
          margin-top: 0;
        }
        ul {
          margin: 0;
          padding-left: 1.2rem;

          li {
          margin: 0;
          list-style: none;
          font-weight: 200;
          line-height: 1.5;
          }
        }
        
      }
    }

  }
</style>
