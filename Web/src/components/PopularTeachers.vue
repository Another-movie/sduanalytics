<template>
  <div class="container">
    <b-field class="select-field" label="Course code">
      <b-input
        v-on:input="onChange()"
        placeholder="HSS 222"
        type="search"
        v-model="selected_course"
        icon="book"
        size="is-medium"
        expanded
      >
      </b-input>
    </b-field>
    <div class="selects">
      <b-field class="select-field" label="Term">
        <b-select
          v-model="selected_term"
          v-on:input="onChange()"
          placeholder="Term"
          size="is-medium"
          expanded
        >
          <option class="custom_option" value="1">1</option>
          <option class="custom_option" value="2">2</option>
          <option class="custom_option" value="3">3</option>
        </b-select>
      </b-field>
      <b-field class="select-field" label="Year">
        <b-select
          v-model="selected_year"
          v-on:input="onChange()"
          placeholder="Year"
          size="is-medium"
          expanded
        >
          <option class="custom_option" value="2016">2016</option>
          <option class="custom_option" value="2017">2017</option>
          <option class="custom_option" value="2018">2018</option>
          <option class="custom_option" value="2019">2019</option>
        </b-select>
      </b-field>
    </div>

    <div class="center">
      <vs-button
        @click="handleClick"
        size="xl"
        :icon="success"
        :upload="loading"
        :color="success ? 'success' : notfound ? 'danger' : 'primary'"
        :disabled="disabled"
        block
      >
        <span v-if="notfound">
          <i class="bx bx-x bx-lg"></i>
        </span>
        <span v-else-if="!success">
          <i class="bx bx-search"></i>
          Search
        </span>
        <span id="success_result" v-else>
          <i class="bx bx-check bx-lg"></i>
        </span>
      </vs-button>
    </div>

    <div class="results">
      <h1 class="results_notfound" v-if="notfound">Not Found</h1>

      <h1 v-else-if="data.length > 0" class="results_h1">Results</h1>
      <ol>
        <li v-for="rec in data.slice((page - 1) * 5, (page - 1) * 5 + 5)">
          <h1>{{ rec.EMP_ID }}</h1>
          <p><b>Students count:</b> {{ rec.REG_COUNT }}</p>
          <p v-if="rec.DIFF">
            The teacher was selected in
            {{ Math.round(+rec.REG_COUNT * +rec.DIFF) }} hours
          </p>
        </li>
      </ol>
      <div class="center" v-if="data.length > 0">
        <vs-pagination
          class="test"
          v-model="page"
          :length="Math.ceil(data.length / 5)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import popular_teachers from "../../database/json/popular_teachers.json";
export default {
  data() {
    return {
      success: false,
      loading: false,
      notfound: false,
      selected_course: "",
      selected_year: "2016",
      selected_term: "1",
      data: [],
      page: 1,
      disabled: true,
    };
  },

  mounted() {},

  methods: {
    onChange() {
      this.success = false;
      this.notfound = false;
      if (
        this.selected_term.length == 0 ||
        this.selected_year.length == 0 ||
        this.selected_course.length == 0
      ) {
        this.disabled = true;
      } else {
        this.disabled = false;
      }
    },
    handleClick() {
      this.page = 1;
      this.loading = true;

      let filtered_data = popular_teachers.filter(
        (val) =>
          val.YEAR == +this.selected_year &&
          val.TERM == +this.selected_term &&
          val.DERS_KOD == this.selected_course.toUpperCase()
      );

      filtered_data.sort((a, b) => {
        if (a.DIFF && b.DIFF) {
          return a.DIFF - b.DIFF;
        } else {
          return b.REG_COUNT - a.REG_COUNT;
        }
      });

      if (filtered_data.length == 0) {
        this.notfound = true;
        this.loading = false;
        this.data = [];
      } else {
        this.notfound = false;
        this.data = filtered_data;
        this.loading = false;
        this.success = true;
      }
    },
  },
};
</script>

<style scoped>
.results_h1 {
  margin-top: 1em;
  font-size: 2rem;
  text-align: center;
}

h1 {
  font-size: 1.2rem;
  font-weight: bold;
  color: #dbdbdb;
}

.results_notfound {
  margin-top: 1em;
  padding-bottom: 1.5em;
  font-size: 2rem;
  text-align: center;
}

.vs-icon-arrow {
  color: white;
}
p,
li {
  color: #adadad;
}
ol {
  padding-bottom: 2em;
}
li {
  list-style: none;
  padding: 1em;
  margin: 1em 0 0 0;
  list-style-position: inside;
  border: 1px solid #333b45;
  border-radius: 4px;
}

.custom_option {
  color: #a2a6ab;
}
#success_result {
  padding: 0.4em;
}
li:hover {
  background-color: #2b333d;
}

.bx bx-check {
}
.center {
  display: flex;
  justify-content: center;
  padding: 1em 0;
}
.selects {
}

.select-field {
  margin: 2em;
}
</style>
