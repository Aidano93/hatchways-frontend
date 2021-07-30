<template>
  <div>
    <div class="student-list-container">
      <div class="student-list-search-wrapper">
        <input
          type="text"
          v-model="searchStudents"
          placeholder="Search by name"
        />
        <input type="text" placeholder="Search by tag" />
      </div>
      <div
        class="student-list"
        v-for="student in filteredStudents"
        :key="student.id"
      >
        <Student :data="student" @addTag="addTag" />
      </div>
    </div>
  </div>
</template>
<script>
import Student from "./Student.vue";
export default {
  name: "Students",
  components: {
    Student,
  },
  data() {
    return {
      students: [],
      searchStudents: "",
      searchTags: "",
    };
  },
  mounted() {
    fetch("https://api.hatchways.io/assessment/students")
      .then((res) => {
        if (!res.ok) throw new Error("Something went wrong");
        return res.json();
      })
      .then((data) => {
        const students = data.students;
        this.students =
          students &&
          students.map((student) => {
            return {
              ...student,
              tags: [],
            };
          });
      })
      .catch(function (error) {
        console.log(error);
      });
  },
  computed: {
    filteredStudents() {
      return this.students.filter((student) => {
        const fullName = student.firstName + " " + student.lastName;
        return fullName
          .toLowerCase()
          .includes(this.searchStudents.toLowerCase());

        // My attempt at getting search by tag to work. I successfully emitted the created tags into the students array but am unable to filter by those tags
        // && student.tags.toLowerCase().includes(this.searchTags.toLowerCase())
      });
    },
  },
  methods: {
    addTag(id, tagField) {
      const student = this.students.find((student) => student.id === id);
      if (student) {
        student.tags.push(tagField);
      }
    },
    check() {
      console.log(this.students[0].tags);
    },
  },
};
</script>
<style lang="scss" scoped>
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
    flex-direction: column;
    position: sticky;
    top: 0;
    z-index: 100;

    input {
      display: block;
      width: inherit;
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
    flex-direction: column;
  }
}

@media screen and (max-width: 785px) {
  .student-list-container {
    margin: 0;
    margin-top: 0;
    margin-bottom: 0;
    width: calc(100vw - 10px);
    height: calc(100vh - 10px);
    overflow: auto;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 10px;
  }
}
</style>