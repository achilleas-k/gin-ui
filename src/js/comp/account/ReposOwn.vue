<!--
    Copyright (c) 2016, German Neuroinformatics Node (G-Node)

    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted under the terms of the BSD License. See
    LICENSE file in the root of the Project.
-->

<template>
    <div>
        <ul class="list-unstyled" v-if="repos">
            <li v-for="repo in repos">
                <div class="panel panel-default">
                    <div class="panel-heading">
                        <router-link :to="{ name: 'repository-info',
                                            params: { username: repo.Owner, repository: repo.Name } }">
                            {{ repo.Owner }}/{{ repo.Name }}
                        </router-link>
                    </div>
                    <div class="panel-body">
                        {{ repo.Description }} <br/> <br/>
                        {{ repo.Public | privacyLabel }}
                    </div>
                </div>
            </li>
        </ul>
        <ul class="list-unstyled" v-if="!repos">
            <li class="panel panel-default">
                <div class="panel-body">
                    There are no available repositories
                </div>
            </li>
        </ul>
    </div>
</template>

<script type="text/ecmascript-6">
    import Alert from "../Alert.js"

    export default {
        data() {
            return {
                repos: null
            }
        },

        mounted() {
            this.update(this.$route.params)
        },

        props: {
            account: { required: true }
        },

        mixins: [ Alert ],

        methods: {
            update(params) {
                const promise_own = api.repos.listUserRepos(params.username)
                promise_own.then(
                        (repos) => {
                            this.repos = repos
                        },
                        (error) => {
                            this.repos = null
                            this.reportError(error)
                        }
                )
            }
        },

        watch: {
            "$route.params": function(params) {
                this.update(params)
            }
        }
    }
</script>
