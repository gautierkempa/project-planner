<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" /> <!-- $event = data qu'on a envoyée -->
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [], // on ajoute les données dans le tableau dans mounted
      current: 'all' // pour les filtres
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
    },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete(id) {
      let p = this.projects.find(project => {
        return project.id === id // true or false (true si match l'id)
      })
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects(){
      if (this.current === 'completed'){
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      return this.projects // pas besoin de if pour afficher TOUS les projets (ordre chronologique)
    }
  }
}

</script>

<style>
.filter-nav button{
  background: none;
  border: none;
  color: #bbb;
  outline: none;
  font-size: 12px;
  text-transform: uppercase;
  margin-right: 10px;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
}
</style>

