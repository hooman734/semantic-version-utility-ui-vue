<template>
    <div>
        <img alt="Vue logo" src="https://th.bing.com/th/id/OIP.04d31jgs0J8aQWHe8wFBtQHaHa?w=173&h=171&c=7&o=5&dpr=1.25&pid=1.7">
        <h2>Please insert a package name to search at the NuGet.org</h2>
        <input type="text" v-model="pkg.name" maxlength="50" size="60" placeholder="Package's name..." v-on:keyup.enter="handleSearch" v-on:keyup.delete="reset">
        <br/>
        <input type="radio" class="radio" id="major" :checked="radio.major" @click="handleMajor">
        <label for="major">Major</label> <br/>
        <input type="radio" class="radio" id="minor" :checked="radio.minor" @click="handleMinor">
        <label for="minor">Minor</label> <br/>
        <input type="radio" class="radio" id="patch" :checked="radio.patch" @click="handlePatch">
        <label for="patch">Patch</label> <br/>
        <br/>
        <div v-show="display.show && pkg.name.length > 0">
            <h4  v-text="pkg.name"></h4>
            <h4 >based on {{pkg.type}}</h4>
            <h4>{{version.major}} . {{version.minor}} . {{version.patch}}</h4>
            <br/>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "NuGet.vue",
        data() {
            return {
                URL: {
                    main: 'http://127.0.0.1:5000/',
                    middle: 'api/nuget/',
                    suffix: '/json'
                },
                pkg: {
                    name: '',
                    type: 'major'
                },
                version: {
                    major: '',
                    minor: '',
                    patch: ''
                },
                radio: {
                    major: true,
                    minor: false,
                    patch: false
                },
                display: {
                    show: false
                }
            }
        },
        methods: {
            handleSearch: function () {
                const {main, middle, suffix} = this.URL;
                const {name, type} = this.pkg;
                let route = main.concat(middle, name, '/', type, '/', suffix);
                axios.get(route).then(reply => {
                this.version.major = reply.data[2][0];
                this.version.minor = reply.data[2][1];
                this.version.patch = reply.data[2][2];
                this.display.show = true;
            }, error => {
                    this.version.major = '-';
                    this.version.minor = '-';
                    this.version.patch = '-';
                console.log(error);
            })
        },
            handleMajor: function () {
                this.pkg.type = 'major';
                this.radio.major = true;
                this.radio.minor = false;
                this.radio.patch = false;
                this.handleSearch();
            },
            handleMinor: function () {
                this.pkg.type = 'minor';
                this.radio.major = false;
                this.radio.minor = true;
                this.radio.patch = false;
                this.handleSearch();
            },
            handlePatch: function () {
                this.pkg.type = 'patch';
                this.radio.major = false;
                this.radio.minor = false;
                this.radio.patch = true;
                this.handleSearch();

            },
            reset: function () {
                if (this.pkg.name.length === 0) {
                    this.version = {major: '', minor: '', patch: ''};
                }
                console.log(this.pkg);
            }
        }
    }

</script>

<style scoped>

</style>