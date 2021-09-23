<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResourceButtonMode">Saved Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResourceButtonMode">Add Resource</base-button>
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

    computed: {
        storedResourceButtonMode(){
            return this.currentSelectedTab === 'stored-resources' ? null : 'flat'
        },
        addResourceButtonMode(){
            return this.currentSelectedTab === 'add-resource' ? null : 'flat'
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
