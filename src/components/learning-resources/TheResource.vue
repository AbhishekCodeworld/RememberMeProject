<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">Stored Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode">Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';
export default{
    components: {
        StoredResources,
        AddResource
    },
    data(){
        return {
            selectedTab : 'stored-resources',
            items: [
            { 
                id: 'officil-guide', 
                title: 'Official Guide',
                description: 'The Official vue.js documentation',
                link: 'https://vuejs.org'
            },
            { 
                id: 'Google', 
                title: 'Google',
                description: 'This is the place where you will find everything.',
                link: 'https://google.com'
            }
        ]
        };
    },
    provide() {
        return {
            items: this.items,
            addResource: this.addResource,
            removeResource: this.removeResource
        };
    },

    computed: {
        storedResButtonMode() {
            return this.selectedTab === 'stored-resources' ? null : 'flat'
        },

        addResButtonMode() {
            return this.selectedTab === 'add-resource' ? null : 'flat';
        }
    },
    methods:{
        setSelectedTab(tab) {
            //console.log(this.selectedTab);
            this.selectedTab = tab;
        },

        addResource(title, description, url){
            const newItem = {
                id: new Date().toISOString(),
                title : title,
                description : description,
                url : url
            };
            this.items.unshift(newItem);
            this.selectedTab = 'stored-resources';
        },

        removeResource(resId){
            const resIndex = this.items.findIndex(res => res.id === resId);
            this.items.splice(resIndex,1);
        }
    }
}
</script>