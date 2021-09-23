<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')">Saved Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')">Add Resource</base-button>
    </base-card>
    <component :is="currentSelectedTab"></component>
    
</template>

<script>
import StoredResources from "./StoredResources.vue";
import AddResource from "./AddResource.vue";
export default ({
    components: {
        StoredResources,
        AddResource
    },
    data() {
        return {
            currentSelectedTab: 'stored-resources',
            storedResources: [
                {
                    id: 'official-guide',
                    title: 'Official Guide',
                    description: 'The official Vue.js documentation',
                    link: 'https://vuejs.org'
                },
                {
                    id: 'google',
                    title: 'Google',
                    description: 'The Google search',
                    link: 'https://google.com'
                },
            ]
        }
    },

    provide() {
        return {
            resources: this.storedResources
        }
    },

    methods: {
        setSelectedTab(tab) {
            this.currentSelectedTab = tab
        }
    }
})
</script>
