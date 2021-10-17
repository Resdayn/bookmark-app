<template>
    <base-card>
        <base-button 
        @click="setSelectedTab('stored-resources'); fetchResources()"
        :mode="storedResourceButtonMode"
        >Saved Resources <loading-spinner v-if="isLoadingFromFirebase"></loading-spinner></base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResourceButtonMode">Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="currentSelectedTab"></component>
    </keep-alive>
    
    
</template>

<script>
import axios from 'axios';
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
            storedResources: [],
            isLoadingFromFirebase: false
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
            removeResource: this.removeResource,
            currentSelectedTab: this.currentSelectedTab // used by AddResource.vue
        }
    },

    methods: {
        setSelectedTab(tab) {
            this.currentSelectedTab = tab
        },
        fetchResources() {
            this.isLoadingFromFirebase = true;
            this.storedResources.splice(0, this.storedResources.length); // empties the array to avoid duplicating items everytime the button is pressed
            axios.get('https://learning-resources-app-a2444-default-rtdb.asia-southeast1.firebasedatabase.app/learning-resources.json')
            .then(response => {
                console.log(`Server response code is ${response.status}`)
                for (const resourceId in response.data) {
                    this.storedResources.push({
                        id: resourceId,
                        title: response.data[resourceId].title,
                        description: response.data[resourceId].description,
                        link: response.data[resourceId].url
                    });
                    }
                    this.isLoadingFromFirebase = false
                })
            .catch(error => console.log(error));
        },

        removeResource(resourceId) {
            // Axios delete request to Firebase
            axios.delete(`https://learning-resources-app-a2444-default-rtdb.asia-southeast1.firebasedatabase.app/learning-resources/${resourceId}.json`)
            .then(response => console.log(response))
            .catch(error => console.log(error.message));
            const resIndex = this.storedResources.findIndex(res => res.id == resourceId);

            // We can't override the array with a filtered array as it would generate a new array which won't be detected by the provide-inject.
            // Instead, we manipulate the original array by deleting the item with the appropriate index
            this.storedResources.splice(resIndex, 1);

        }
    }
})
</script>
