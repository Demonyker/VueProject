<template>
    <div>
        <div class="search-place">
                <div class="search-place__type">
                    <div class="types-list">
                        <p class="currentType">Type</p>
                    </div>
                    <div class="black-arrow"></div>
                </div>
                <div class="search-place__language" v-on:click="openLanguagesList">
                    <div class="search-place__language-list">
                        <p class="currentLanguage">{{ currentLanguage }}</p>
                        <ul class="list" v-if="listOfLanguagesOpen">
                          <li class="list__item"
                          v-for="(language, i) of languages"
                          v-bind:key="i"
                          v-on:click="$emit('chooseLanguage', language)"
                          >
                          {{ language }}
                          </li>
                        </ul>
                    </div>
                    <div class="black-arrow"></div>
                </div>
                <div class="input-style">
                    <input type="text" class="search-place__input"
                    v-model="inputTopic"
                    />
                    <p class="input-style-type"
                    v-bind:class="{'input-style-type-active': inputTopic.length}">Type here for search</p>
                </div>
                <button class="search-place__button"
                v-on:click="$emit('apiRequest', currentLanguage, inputTopic)"
                >
                SEARCH</button>
        </div>
    <repositoriesList
    v-bind:repositories="repositories"
    v-bind:keyWords="keyWords"
    v-bind:viewStatusList="viewStatusList"
    v-bind:viewStatusBar="viewStatusBar"
    v-on:changeView="$emit('changeView')"
    v-on:addRepoToMyList="addRepoToMyList"
    />
    </div>
</template>

<script>
// @ is an alias to /src
/* eslint-disable */
import repositoriesList from '@/components/repositoriesList.vue';

export default {
  name: 'home',
  data() {
      return {
        languages: ['javascript', 'css', 'html', 'php', 'ruby', 'c++', 'python', 'c#', 'java', 'go', 'haskel'],
        types: ['Repositories'],
        listOfLanguagesOpen: false,
      }
  },
  props: ['currentLanguage', 'inputTopic', 'repositories', 'keyWords', 'viewStatusList', 'viewStatusBar', 'myRepositories'],
  components: {
      repositoriesList
  },
  methods: {
    openLanguagesList() {
          this.listOfLanguagesOpen = !this.listOfLanguagesOpen;
    },
    addRepoToMyList(repo) {
        this.$emit('addRepoToMyList', repo)
    }
  }
};
</script>

<style lang="scss">
.search-place {
    display: flex;
    width: 956px;
    margin: 0 auto;
    margin-top: 34px;
    height: 36px;
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 14.5px;
    line-height: 17px;
    max-width: 100%;
    &__type {
        width: 205px;
        border-bottom: 1px solid #0366D6;
        margin-right: 25px;
        display: flex;
        justify-content: space-between;
        cursor: pointer;
    }
    &__language {
        position: relative;
        width: 205px;
        border-bottom: 1px solid #0366D6;
        margin-right: 25px;
        cursor: pointer;
        display: flex;
        justify-content: space-between;
    }
    &__input {
        width: 336px;
        border: none;
        border-bottom: 1px solid #0366D6;
        margin-right: 25px;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        padding-bottom: 6px;
    }
    &__button {
        width: 129px;
        height: 36px;
        background: #0366D6;
        box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.25);
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        color: #FFFFFF;
    }
}
.list {
    width: 203px;
    display: block;
    list-style-type: none;
    padding: 0px;
    border-left: 1px solid #0366D6;
    border-right: 1px solid #0366D6;
    margin-top: 9px;
    position: relative;
    z-index: 4;
    &__item {
        background: white;
        border-bottom: 1px solid #0366D6;
        cursor: pointer;
        padding: 5px;
    }
}
.languages-list-open {
    display: block;
}
.types-list-open {
    display: block;
}
.currentLanguage {
    margin-top: 10px;
    margin-bottom: 0px;
    padding-left: 5px;
}
.currentType {
    margin-top: 10px;
    margin-bottom: 0px;
}
.black-arrow {
    width: 20px;
    height: 20px;
    background: url('./img/arrow-bottom.svg') no-repeat;
    margin-top: 10px;
}
.search-place__language-list {
    width: 100px;
}
.types-list {
    width: 100px;
}
input {outline:none;}
::-webkit-input-placeholder { color: black; }
::-moz-placeholder          { color: black; } /* Firefox 19+ */
:-moz-placeholder           { color: black; } /* Firefox 18- */
:-ms-input-placeholder      { color: black; }
.search-hide {
    display: none;
}
.input-style {
    position: relative;
    padding-top: 11px;
    &-type {
        position: absolute;
        left: 0;
        top: 0;
        pointer-events: none;
        transition: ease .5s;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        margin-top: 11px;
    }
}
.search-place__input:focus+.input-style-type {
    top: -15px;
    color: #0366D6;
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 14px;
}
.input-style-type-active {
    top: -15px;
    color: #0366D6;
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 14px;
}
</style>
