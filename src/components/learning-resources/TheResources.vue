<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedRes">Stored Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addRes">Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
import AddResource from './AddResource.vue';
import StoredResources from './StoredResources.vue';

export default {
    components : {
        StoredResources,
        AddResource
    },
    data() {
        return {
            selectedTab : 'stored-resources',
            storedResources : [
                {
                    id          : 'offical-guide',
                    title       : 'Official Guide',
                    description : 'The official Vue js documentation',
                    link        : 'https://vuejs.org',
                },
                {
                    id          : 'google',
                    title       : 'Google',
                    description : 'Learn to Google...',
                    link        : 'https://google.com',
                },
            ]
        }
    },

    provide() {
        return {
            resources   : this.storedResources,
            addResource : this.addResource,
            removeResource : this.removeResource
        }
    },

    computed : {
        storedRes() {
            return this.selectedTab === 'stored-resources' ? null : 'flat'
        },
        addRes() {
            return this.selectedTab === 'add-resource' ? null : 'flat'
        }
    },

    methods : {
        setSelectedTab(tab) {
            this.selectedTab = tab;
        },
        addResource(title, description, link) {
            const newResource = {
                id : new Date().toISOString(),
                title, 
                description,
                link
            }

            this.storedResources.unshift(newResource);
            this.selectedTab = 'stored-resources';
        },
        removeResource(resourceId) {
            const resIndex = this.storedResources.findIndex(res => res.id === resourceId);
            this.storedResources.splice(resIndex, 1);
        }
    }
}
</script>
