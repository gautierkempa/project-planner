<template>
  <div class="project" :class="{ complete: project.complete}">
      <div class="actions">
          <h3 @click="showDetails =! showDetails"> {{ project.title }}</h3> <!-- montrera les détails s'ils sont pas et inversément -->
          <div class="icons">
              <span @click="toggleComplete" class="material-icons tick">
                done
              </span>
              <router-link :to="{ name: 'EditProject', params: { id: project.id}}">
                <span class="material-icons">edit</span>
              </router-link>
              
              <span @click="deleteProject" class="material-icons">
                delete
              </span>
          </div>
      </div>
      <div v-if="showDetails" class="details">
          <p>{{ project.details }}</p>
      </div>
  </div>
</template>

<script>
export default {
    props: ['project'],
    data() {
        return {
            showDetails: false, // permet d'afficher au click
            uri: 'http://localhost:3000/projects/' + this.project.id // endpoint
        }
    },
    methods: {
      deleteProject(){
        fetch(this.uri, { method: 'DELETE' }) // delete request to the json server
        .then(() => this.$emit('delete', this.project.id)) // permet de supprimer les data localement
        .catch(err => console.log(err))
      },
      toggleComplete(){
        fetch(this.uri, {
           method: 'PATCH',
           headers: { 'Content-type': 'application/json' },
           body: JSON.stringify({ complete: !this.project.complete}) // met l'inverse du complete
        }) // méthode patch permet de modifier un élément à l'intérieur de l'objet et pas tout l'objet
        .then(() => {
          this.$emit('complete', this.project.id)
        }).catch((err) => console.log(err))
      }
    }
}
</script>

<style>
.project{
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid red;
}
h3{
    cursor: pointer;
}

.actions{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-icons{
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover{
  color: #777;
}

.project.complete {
  border-left: 4px solid #00ce89;
}
.project.complete .tick{
  color: #00ce89
}
</style>