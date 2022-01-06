<template>
    <base-card>
        <base-button 
        @click="setSelectedTab('stored-resources'); fetchResources()"
        :mode="storedResourceButtonMode"
        ><img src="../../../public/icons/load.png"> Load Saved Bookmarks <loading-spinner v-if="isLoadingFromFirebase"></loading-spinner></base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResourceButtonMode"><img src="../../../public/icons/add.png"> Add Bookmark</base-button>
    </base-card>
    <keep-alive>
        <component :is="currentSelectedTab"></component>
    </keep-alive>
    
    
</template>

<script>
import axios from 'axios';
import StoredResources from "./StoredResources.vue";
import AddResource from "./AddResource.vue";
import fireBaseToken from "../../../token.js"

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
            removeResource: this.removeResource,
        }
    },

    methods: {
        setSelectedTab(tab) {
            this.currentSelectedTab = tab
        },
        fetchResources() {
            this.isLoadingFromFirebase = true;
            this.storedResources.splice(0, this.storedResources.length); // empties the array to avoid duplicating items everytime the button is pressed
            axios.get(`${fireBaseToken.token}/learning-resources.json`)
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
            axios.delete(`${fireBaseToken.token}/learning-resources.json/${resourceId}.json`)
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

<style scoped>
img {
    transform: scale(0.8);
    position: relative;
    top: 0.4rem;
}
</style>