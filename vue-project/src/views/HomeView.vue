<template>
  <div class="main">
    <div class="header">
      <h1>Animals</h1>
      <p>Search: <input type="text" v-model="search" placeholder="Click Me..."></p>
      <button class="endangered" @click="currentIndex++">Status: {{ endageredList[currentIndex] }}</button>
    </div>
    <List :list="list" :ammount="showAnimals" />
    <button v-if="list.length >= showAnimals" @click="showAnimals += 99">Show More</button>
  </div>
</template>
<script>
import List from "../components/ListComp.vue"
export default {
  components: {
    List
  },
  data() {
    return {
      data: [],
      animal: "",
      showAnimals: 99,
      search: "",
      list: [],
      endageredList: ["All"],
      currentIndex: 0,
    }
  },
  methods: {
    async callApi() {
      this.data = await fetch('https://ecos.fws.gov/ecp/pullreports/catalog/species/report/species/export?columns=/species;/species/speciesImage')
        .then(data => data.json())
      console.log(await this.data)
      let duplicates = [];
      this.data.data = await this.data.data.filter(value => {
        if (!this.endageredList.includes(value[4])) this.endageredList.push(value[4])
        if (duplicates.includes(value[0])) return false
        else {
          duplicates.push(value[0])
          return true
        }
      })
      this.list = await this.data.data.filter(value => {
        if (this.search.length !== 0) {
          return value[0].toLowerCase().includes(this.search.toLowerCase()) || value[2].value.toLowerCase().includes(this.search.toLowerCase())
        } else { return true }
      })
      console.log(this.endageredList)
    }
  },
  computed: {
    filterData() {
      this.list = this.data.data.filter(value => {
        if (value[4] !== this.endageredList[this.currentIndex] && this.endageredList[this.currentIndex] !== "All") {
          return false
        } else if (this.search.length !== 0) {
          return value[0].toLowerCase().includes(this.search.toLowerCase()) || value[2].value.toLowerCase().includes(this.search.toLowerCase())
        }
        else { return true }
      })

    }
  },
  watch: {
    search(newVal, oldVal) {
      this.filterData
    },
    currentIndex(newVal, oldVal) {
      if (this.currentIndex >= this.endageredList.length) this.currentIndex = 0
      this.filterData
    }
  },
  mounted() {
    this.callApi()
  }
}
</script>


<style scoped>
.main {
  padding-bottom: 5rem;
  ;
}

.header {
  height: 7rem;
  background-color: rgb(255, 129, 129);
  margin: auto;
  border-radius: 30px;
  text-align: center;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

h1 {
  color: black;
  font-weight: 900;
  font-size: 5rem;
}

input {
  font-size: 14px;
  border-radius: 6px;
  line-height: 1.5;
  padding: 5px 10px;
  transition: box-shadow 100ms ease-in, border 100ms ease-in, background-color 100ms ease-in;
  border: 2px solid #dee1e2;
  color: rgb(14, 14, 16);
  background: #dee1e2;
  height: 36px;
}

p {
  font-size: 1.5rem;
  vertical-align: center;
}

input:hover {
  border-color: #ccc;
}

input:focus {
  border-color: #9147ff;
  background: #fff;
}

p {
  color: black;
}

.endangered {
  height: 4rem;
  width: 20rem;
}

button {
  width: 100%;
  display: inline-block;
  outline: none;
  cursor: pointer;
  font-size: 16px;
  line-height: 20px;
  font-weight: 600;
  border-radius: 8px;
  padding: 14px 24px;
  border: none;
  transition: box-shadow 0.2s ease 0s, -ms-transform 0.1s ease 0s, -webkit-transform 0.1s ease 0s, transform 0.1s ease 0s;
  background: linear-gradient(to right, rgb(230, 30, 77) 0%, rgb(227, 28, 95) 50%, rgb(215, 4, 102) 100%);
  color: #fff;

}
</style>