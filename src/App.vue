<!-- eslint-disable -->
<template>
  <div id="app">
    <div class="header">
      <div class="header__logo">
          <svg class="git-logo" width="42" height="48" viewBox="0 0 42 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M20.4282 24.5104C23.8694 24.1027 27.5295 22.8475 27.5295 16.8846C27.5295 15.1898 26.9346 13.8086 25.9656 12.7414C26.1219 12.3338 26.622 10.7958 25.777 8.66203C25.777 8.66203 24.5257 8.25386 21.5857 10.2308C20.3344 9.88597 18.9581 9.6659 17.6756 9.6659C16.3612 9.6659 14.9845 9.88597 13.7644 10.2308C10.7925 8.25386 9.54127 8.66203 9.54127 8.66203C8.69685 10.7958 9.19753 12.3338 9.35378 12.7414C8.3838 13.8085 7.78944 15.1898 7.78944 16.8846C7.78944 22.8475 11.4495 24.1027 14.8907 24.5104C14.4531 24.8877 14.0463 25.6088 13.89 26.6133C13.0137 26.9896 10.73 27.7118 9.35378 25.3266C9.35378 25.3266 8.54012 23.8519 7.00752 23.7578C7.00752 23.7578 5.47444 23.7265 6.88245 24.6985C6.88245 24.6985 7.91444 25.1699 8.6031 26.9582C8.6031 26.9582 9.54127 29.9401 13.8582 28.9983V31.6657C13.8582 32.0744 13.5457 32.5447 12.7638 32.4193C6.63198 30.3791 2.22126 24.5416 2.22126 17.7004C2.22126 9.13241 9.13448 2.1971 17.6756 2.1971C26.1844 2.1971 33.1294 9.13248 33.0981 17.7004C33.0981 24.5416 28.6869 30.3477 22.5556 32.4193C21.7732 32.5761 21.4606 32.1058 21.4606 31.6657V27.3668C21.4607 25.9223 20.9908 24.9817 20.4282 24.5104ZM35.3194 17.7322C35.3194 7.93981 27.4045 6.72444e-08 17.6756 6.72444e-08C7.91444 6.72444e-08 -7.45072e-07 7.93981 -7.45072e-07 17.7322C-7.45072e-07 27.4923 7.91444 35.4326 17.6756 35.4326C27.4045 35.4326 35.3194 27.4923 35.3194 17.7322Z" fill="white"/>
            <path d="M27.0881 32.58C27.0881 32.58 31.4392 43.6471 37.2649 47.744C38.8177 48.836 42.6521 46.1892 41.9043 44.4409C39.366 38.5064 29.6323 30.1778 29.6323 30.1778L27.0881 32.58Z" fill="white"/>
            </svg>
            <div class="header__logo-text">GitHub <br> <p class="header__logo-text-search">search</p></div></div>
      <div class="header__pages">
        <router-link to="/search" class="search-page" active-class="search-page-active"> Search</router-link>
        <router-link to="/my-list" class="mylist-page" active-class="mylist-page-active">My list</router-link>
      </div>
    </div>
    <router-view :currentLanguage="currentLanguage"
    v-on:chooseLanguage="chooseLanguage"
    :inputTopic="inputTopic"
    v-on:apiRequest="apiRequest"
    :repositories="repositories"
    :myRepositories="myRepositories"
    :keyWords="keyWords"
    :viewStatusList="viewStatusList"
    :viewStatusBar="viewStatusBar"
    v-on:changeView="changeView"
    v-on:addRepoToMyList="addRepoToMyList"
    />
    <div class="footer"><p>copyright @lodossteam 2018</p></div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      test: 'test',
      repositories: [],
      myRepositories: [],
      currentLanguage: 'Language',
      inputTopic: '',
      notFoundResults: false,
      keyWords: [],
      viewStatusList: true,
      viewStatusBar: false, 
    };
  },
  methods: {
    addRepoToMyList(repo) {
        if (repo.addedStatus === false) {
            repo.addedStatus = true;
            this.myRepositories.push(repo);
        } else if (repo.addedStatus) {
            repo.addedStatus = false;
            this.repositories.find((currentRepo) => {
              if (currentRepo.id === repo.id) {
                currentRepo.addedStatus = false;
              }
            })
            this.myRepositories = this.myRepositories.filter(repos => {
                return repos.id !== repo.id;
            })
        }
    },
    changeView() {
      this.viewStatusList = !this.viewStatusList;
      this.viewStatusBar = !this.viewStatusBar;
    },
    chooseLanguage(language) {
      this.currentLanguage = language;
    },
    apiRequest(languages, topics) {
      this.repositories.length = 0;
      this.keyWords.length = 0;
      this.inputTopic = topics;
      this.notFoundResults = (this.currentLanguage === 'Language');
      fetch(`https://api.github.com/search/repositories?q=language:${languages}+topic:${topics}`)
      .then( response => {
        return response.json();
      })
      .then( results => {
        if (results.items.length === 0) {
          this.notFoundResults = true;
        } else if(results.items.length !== 0 && this.currentLanguage !== 'Language') {
          this.keyWords.push(results.items[0].language);
          this.notFoundResults = false;
        }
        if (topics !== '') {
          topics.split(' ').forEach(topic => {
            this.keyWords.push(topic);
          })
        }
        results.items.forEach(repo => {
          repo.addedStatus = false;
          this.myRepositories.find(currentRepo => {
            if (currentRepo.id === repo.id) {
              repo.addedStatus = true;
            }
          });
          this.repositories.push(repo);
        });
      });
    },
  },
};
</script>

<style lang="scss">
#app {
  position: relative;
  min-height: 100vh;
}
.header {
    background: #3C4146;
    width: 100%;
    display: flex;
    color: white;
    justify-content: space-between;
    font-family: Roboto;
    height: 60px;
    &__logo {
       display: flex;
       font-style: normal;
       font-weight: bold;
       font-size: 24px;
       margin-left: 30px;
       height: auto;
    }
    &__logo-text {
        margin-top: 10px;
        margin-left: 16px;
    }
    &__logo-text-search {
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 16px;
        color: #B3BCC7;
        margin-top: 0px;
    }
    &__pages {
        display: flex;
        margin-right: 30px;
        font-family: Roboto;
        font-style: normal;
        font-weight: bold;
        font-size: 16px;
        line-height: 19px;
        align-items: center;
    }
}
.git-logo {
    margin-top: 6px;
}
.search-page {
    margin-right: 19px;
}
.search-page, .mylist-page {
    color: #B3BCC7;
    cursor: pointer;
    text-decoration: none;
}
.search-page-active, .mylist-page-active {
    color: white;
    text-decoration: none;
}
.footer {
    position: absolute;
    bottom: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 40px;
    background: #3C4146;
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    line-height: 17px;
    color: #FFFFFF;
}
</style>
