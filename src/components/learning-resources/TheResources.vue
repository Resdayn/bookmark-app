<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResourceButtonMode">Saved Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResourceButtonMode">Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="currentSelectedTab"></component>
    </keep-alive>
    
    
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
            resources: this.storedResources,
            addResource: this.addResource, // this will be used by AddResource.vue
            removeResource: this.removeResource
        }
    },

    methods: {
        setSelectedTab(tab) {
            this.currentSelectedTab = tab
        },
        addResource(title, description, url) {
            const newResource = {
                id: new Date().toISOString(),
                title: title,
                description: description,
                link: url,
            };
            this.storedResources.unshift(newResource);
            this.currentSelectedTab = 'stored-resources';
        },
        removeResource(resId) {
            // For this one, we can't override the array with a filtered array as it would generate a new array which won't be detected by the provide-inject.
            // Instead, we manipulate the original array by deleting the item with the appropriate index

            const resIndex = this.storedResources.findIndex(res => res.id == resId);

            this.storedResources.splice(resIndex, 1);

        }
    }
})
</script>
