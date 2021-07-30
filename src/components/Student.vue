<template>
  <div class="student-list-wrapper">
    <div class="student-list-main">
      <div class="student-list-image">
        <img
          :src="student.pic"
          :alt="'portrait of ' + student.firstName + ' ' + student.lastName"
        />
      </div>
      <div class="student-list-info">
        <h3>{{ `${student.firstName} ${student.lastName}` }}</h3>
        <ul>
          <li>Email: {{ student.email }}</li>
          <li>Company: {{ student.company }}</li>
          <li>Skill: {{ student.skill }}</li>
          <li>Average: {{ getAverage(student.grades) }}%</li>
        </ul>
        <div class="tag-list">
          <span v-for="(tag, i) in student.tags" :key="i">{{ tag }}</span>
        </div>
        <input
          class="tag-input"
          type="text"
          v-model="tagField"
          @keyup.enter="addTag()"
          placeholder="Add a tag"
        />
        <div v-if="expanded" class="grade-list">
          <ul>
            <li v-for="(grades, index) in student.grades" :key="grades">
              test {{ index + 1 }}: {{ grades }}
            </li>
          </ul>
        </div>
      </div>
    </div>
    <button
      class="expand-btn"
      v-if="!expanded"
      @click="expanded = !expanded"
    ></button>
    <button class="showMinus" v-else @click="expanded = !expanded"></button>
  </div>
</template>
<script>
export default {
  name: "Student",
  props: {
    data: Object,
  },
  data() {
    return {
      student: {
        id: this.data.id,
        firstName: this.data.firstName,
        lastName: this.data.lastName,
        email: this.data.email,
        company: this.data.company,
        pic: this.data.pic,
        skill: this.data.skill,
        grades: this.data.grades,
        tags: this.data.tags || [],
      },
      tagField: "",
      expanded: false,
    };
  },
  methods: {
    getAverage(arr) {
      let sum = 0;
      for (let i = 0; i < arr.length; i++) {
        sum += parseInt(arr[i], 10);
      }
      const avg = sum / arr.length;
      return avg;
    },
    expand(student) {
      const index = this.expanded.indexOf(student);
      if (index >= 0) {
        this.expanded.splice(index, 1);
      } else {
        this.expanded.push(student);
      }
    },
    addTag() {
      if (!this.tagField) return;
      this.$emit("addTag", this.student.id, this.tagField);
      this.tagField = "";
    },
  },
};
</script>
<style lang="scss" scoped>
.student-list-wrapper {
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
      .tag-list {
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        padding-left: 1.2rem;

        span {
          padding: 0.5rem;
          margin: 0.2rem;
          background-color: $light-grey;
          border-radius: 5px;
        }
      }
      .tag-input {
        outline: none;
        border: none;
        border-bottom: 1px solid $light-grey;
        margin-top: 1rem;
        padding-left: 1rem;
      }
      .grade-list {
        margin-top: 0.75rem;
      }
    }
  }

  .expand-btn {
    background: transparent;
    background-image: url(../assets/plus-solid.svg);
    background-size: contain;
    background-repeat: no-repeat;
    width: 2rem;
    height: 2rem;
    border: none;
    cursor: pointer;
    opacity: 0.4;
    margin-top: 0.45rem;

    &:hover {
      opacity: 1;
    }
  }

  .showMinus {
    background: transparent;
    background-image: url(../assets/minus-solid.svg);
    background-size: contain;
    background-repeat: no-repeat;
    width: 2rem;
    height: 2rem;
    border: none;
    cursor: pointer;
  }
}
@media screen and (max-width: 785px) {
  .student-list-wrapper {
    flex-direction: column;

    .student-list-info {
      h3 {
        font-size: 1.9rem !important;
      }
      ul {
        padding-left: 0 !important;
      }
    }
  }
}
</style>