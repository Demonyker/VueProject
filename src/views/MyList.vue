<!-- eslint-disable -->
<template>
    <div>
        <p class="my-repositories-list-active">My List</p>
        <repositoriesList
        v-bind:repositories="myRepositories"
        v-bind:keyWords="keyWords"
        v-bind:viewStatusList="viewStatusList"
        v-bind:viewStatusBar="viewStatusBar"
        v-on:changeView="$emit('changeView')"
        v-on:addRepoToMyList="addRepoToMyList"
        />
    </div>
</template>

<script>

import repositoriesList from '@/components/repositoriesList.vue';

export default {
    components: {
      repositoriesList
    },
    props: ['currentLanguage', 'inputTopic', 'repositories', 'keyWords', 'viewStatusList', 'viewStatusBar', 'myRepositories'],
    methods: {
        addRepoToMyList(repo) {
            this.$emit('addRepoToMyList', repo)
        }
    }
}
</script>

<style lang="scss">
    .my-repositories-list-active {
        display: block;
        width: 116px;
        height: 43px;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 36px;
        line-height: 42px;
        color: #000000;
        margin-bottom: 0px;
        width: 956px;
        margin: 0 auto;
        max-width: 100%;
        margin-top: 35px;
    }
</style>