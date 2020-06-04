<template>
    <div>
        <h1>{{majorVersion}}</h1>
        <h1>{{minorVersion}}</h1>
        <h1>{{patchVersion}}</h1>
        <button formaction="submit"  v-on:click=="handleSearch"></button>
    </div>
</template>

<script>
    import axios from 'axios'
    // import json from 'json'
    export default {
        name: "NuGet.vue",
        data() {
            return {
                majorVersion: 'hgghghggg',
                minorVersion: '',
                patchVersion: ''
            }
        },
        methods: {
            handleSearch: function () {
            axios.get('http://127.0.0.1:5000/api/nuget/lion.core/minor/json').then(reply => {

                this.majorVersion = reply.data[2][0];
                this.minorVersion = reply.data[2][1];
                this.patchVersion = reply.data[2][2];
                console.log(reply.data[2]);
            }, error => {
                console.log(error);
            })
                return "is done";
        }
        }
    }

</script>

<style scoped>

</style>