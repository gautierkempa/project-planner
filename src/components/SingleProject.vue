<template>
  <div class="project">
      <div class="actions">
          <h3 @click="showDetails =! showDetails"> {{ project.title }}</h3> <!-- montrera les détails s'ils sont pas et inversément -->
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
            showDetails: false // permet d'afficher au click
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
</style>