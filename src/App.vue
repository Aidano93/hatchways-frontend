<template>
  <div class="student-list-container">
    <div class="student-list-search-wrapper">
      <input type="text" v-model="search" placeholder="Search by name">
    </div>
    <div class="student-list" v-for="student in filteredStudents" :key="student.id">
      <div class="student-list-main">
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
          <div class="student-list-grades-min" :class="{expanded: expanded.includes(student)}" >
            <ul v-for="(grades, index) in student.grades" :key="grades">
              <li>test {{index+1}}: {{grades}}</li>
            </ul>
          </div>
        </div>
      </div>
      <button class="expand-btn" :class="{showMinus: expanded.includes(student)}"  @click="expand(student)"></button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return{
      students: [],
      expanded: [],
      search: '',
      showList: false,
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
  computed: {
    filteredStudents() {
      return this.students.filter( student => {
        return student.firstName.toLowerCase().includes(this.search.toLowerCase()) || student.lastName.toLowerCase().includes(this.search.toLowerCase());
      })
    }
  },
  methods: {
    getAverage(arr) {
      let sum = 0;
      for( let i = 0; i < arr.length; i++ ){
      sum += parseInt( arr[i], 10 );
      }
      const avg = sum/arr.length;
      return avg
    },
    expand(student){
      const index = this.expanded.indexOf(student)
      if (index >= 0) {
        this.expanded.splice(index,1)
      } else {
        this.expanded.push(student)
      }
    },
  },
}
</script>

<style lang="scss">

  .student-list-container {
    margin: auto;
    margin-top: 5rem;
    margin-bottom: 5rem;
    width: 60vw;
    height: 82vh;
    overflow: auto;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 10px;

    &::-webkit-scrollbar {
      background: transparent;
      width: 0.5rem;
    }

    &:hover {
      overflow-y: scroll;
      
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

    .student-list-search-wrapper {
      display: flex;
      position: sticky;
      top: 0;
      z-index: 100;

      input {
        display: block;
        width: 100%;
        border: none;
        border-bottom: 1px solid black;
        font-size: 1rem;
        padding: 1rem 0.5rem;
        margin: 0 0.5rem;
        outline: none;
      }
    }

    .student-list {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      padding: 1rem;
      border-bottom: $border-bottom;
      
      .student-list-main {
        display: flex;

        .student-list-image {
          display: flex;
          justify-content: center;
          align-self: flex-start;

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
          
          .student-list-grades-min {
            display: none;
          }
          .expanded {
            display: block;
          }
        }
      }

      .expand-btn {
        background: transparent;
        background-image: url(assets/plus-solid.svg);
        background-size: contain;
        background-repeat: no-repeat;
        width: 2rem;
        height: 2rem;
        border: none;
        cursor: pointer;
        opacity: 0.4;

        &:hover {
          opacity: 1;
        }
      }

      .showMinus {
        background: transparent;
        background-image: url(assets/minus-solid.svg);
        background-size: contain;
        background-repeat: no-repeat;
        width: 2rem;
        height: 2rem;
        border: none;
        cursor: pointer;  
      }
    }

  }
</style>
