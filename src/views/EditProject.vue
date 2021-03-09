<template>
  <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input type="text" v-model="title" required> 
      <label>Details:</label>
      <textarea v-model="details" required></textarea>
      <button>Update Project</button>
  </form>
</template>

<script>
export default {
    props: ['id'],
    data(){
        return {
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/' + this.id
        }
    },
    mounted(){
        fetch(this.uri)
        .then(res => res.json())
        .then(data => {
            this.title = data.title, // on met à jour nos data avec les data qu'on récupère (celle du projet sur lequel on a cliqué)
            this.details = data.details
        })
    },
    methods: {
        handleSubmit() {
            fetch(this.uri, {
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({title: this.title, details: this.details})
            })
            .then(() => {
                this.$router.push('/')
            }).catch (err => console.log(err))
        }
    }
}
</script>

<style>

</style>