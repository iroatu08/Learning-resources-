//tab that switches between add resources and stored resources 
<template>
    <base-card>
    <base-button :mode="storeChangeStyle" @click="SetSelectedTab('stored-resources')">Stored Resources</base-button>
    <base-button :mode="addChangeStyle"  @click="SetSelectedTab('add-resource')" >Add Resource</base-button>
    </base-card>
<keep-alive>
    <component :is="selectedTab" ></component>
</keep-alive>

</template>

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
    components:{
        StoredResources,
        AddResource
    },

    computed: {
        storeChangeStyle(){
            if(this.selectedTab === 'stored-resources' ){ 
                return null
            }else{return 'flat'}
        },

        addChangeStyle(){
            if(this.selectedTab === 'add-resource' ){ 
                return null
            }else{return 'flat'}
        }
    },

   data(){
    return{
        selectedTab: 'stored-resources',
        storedResources: [
            {
                id: 'official-guide', 
                title: 'Official Guide',
                description: 'The official vue js documentation',
                link: 'https://vuejs.org'
            
            },
            {
                id: 'google', 
                title: 'Google',
                description: 'Learn how to google',
                link: 'https://google.com'
            
            }
            ]
    };
   },
   provide(){
    return{
        resources: this.storedResources,
        addResource : this.addResource,
        deleteResource : this.removeResource
    };
   },

   methods: {
    SetSelectedTab(tab){
        this.selectedTab = tab
    },

    addResource(title, description, url){
        const newResource = {
            id: new Date().toISOString(),
            title: title,
            description: description,
            link: url
        };
        this.storedResources.unshift(newResource);
        this.selectedTab = 'stored-resources';
    },

    removeResource(resId){
        // this.storedResources = this.storedResources.filter(res => res.id !== resId);
        const resIndex = this.storedResources.findIndex(res => res.id === resId);
        this.storedResources.splice(resIndex, 1);

    }
   }
}
</script>