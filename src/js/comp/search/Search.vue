<!--
    Copyright (c) 2016, German Neuroinformatics Node (G-Node)

    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted under the terms of the BSD License. See
    LICENSE file in the root of the Project.
-->

<template>
    <div>
        <h2>Search</h2>
        <hr />
        <div class="form-inline row">
            <div class="form-group col-sm-9 col-sm-offset-1">
                <label class="sr-only" for="search">Search</label>
                <input type="text" class="form-control" id="search"
                       placeholder="Search Text" v-model="search_text"
                       style="width: 100%">
            </div>
        </div>
        <hr />

        <div>
            <div v-if="search_text">
                <ul class="nav nav-tabs">
                    <li role="presentation" :class="{ 'active': $route.name === 'search-repos' }">
                        <router-link :to="{ name: 'search-repos' }">
                            Repositories ({{ public_repo.length }})
                        </router-link>
                    </li>
                    <li role="presentation" :class="{ 'active': $route.name === 'search-users' }">
                        <router-link :to="{ name: 'search-users' }">
                            Users ({{ users.length }})
                        </router-link>
                    </li>
                </ul>

                <router-view v-bind:users="users"
                             v-bind:public_repo="public_repo"></router-view>
            </div>
            <div v-if="!search_text">
                Search for public repositories or users
            </div>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">

    import Alert from "../Alert.js"

    export default {
        data() {
            return {
                users: null,
                search_text: null,
                public_repo: null
            }
        },

        mounted() {
            this.search()
        },

        methods: {
            search() {

                const all_public = api.repos.listPublic()
                all_public.then(
                        (repos) => {
                            const filter_public = api.repos.filterRepos(this.search_text, repos)
                            filter_public.then(
                                    (repos) => {
                                        this.public_repo = repos
                                    }
                            )
                        },
                        (error) => {
                            this.reportError(error)
                        }
                )

                const user_search = api.accounts.search(this.search_text)
                user_search.then(
                        (users) => {
                            this.users = users
                        }
                )

                Promise.all([all_public, user_search]).then(
                        () => {
                            if (this.search_text) {
                                var tab = "search-repos"
                                if ((this.public_repo.length == 0) && (this.users.length > 0)) {
                                    tab = "search-users"
                                }
                                this.$router.push({
                                    name: tab
                                })
                            }
                        },
                        (error) => {
                            this.reportError(error)
                        }
                )
            }
        },

        mixins: [Alert],

        watch: {
            search_text: function() {
                this.search()
            }
        }
    }
</script>
