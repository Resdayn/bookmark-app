<template>
<!-- Created to remove code from App.vue and to be able to cleanly modify the <ul> outside of App.vue -->
<!-- This receives the list of resources from App and generate the learning resources by sending the props there -->
    <ul>
        <learning-resource 
        v-for="resource in resources" :key="resource.id"
        :title="resource.title"
        :description="resource.description"
        :link="resource.link">
        </learning-resource>
    </ul>
</template>

<script>
import LearningResource from "./LearningResource.vue"
export default {
    components: {
        LearningResource
    },
    inject: ['resources']
}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin: auto;
  max-width: 40rem;
}
</style>

