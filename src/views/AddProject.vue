<template>
  <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input type="text" v-model="title" required> 
      <label>Details:</label>
      <textarea v-model="details" required></textarea>
      <button>Add project</button>
  </form>
</template>

<script>
export default {
    data() {
        return{
            title: '',
            details: ''
        }
    },
    methods: {
        handleSubmit(){
            let project = {
                title : this.title, // pas besoin d'add l'id, ça va se faire tout seul
                details: this.details,
                complete: false
            }
            fetch('http://localhost:3000/projects', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' }, // permet de dire que c'est du json
                body: JSON.stringify(project) // faut utiliser la méthode stringify pour envoyer du json et passer le nom de l'objet entre ()
            }).then(() => {
                this.$router.push('/')
            }).catch((err) => console.log(err))
        }
    }
}
</script>

<style>
form{
    background: white;
    padding: 20px;
    border-radius: 10px;
}

label{
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
}

input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
}
textarea{
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
}
form button{
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
}

</style>