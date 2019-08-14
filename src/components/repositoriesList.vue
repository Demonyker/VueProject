<!-- eslint-disable -->
<template>
  <div>
    <div class="repositories-list" v-if="viewStatusList">
      <div class="change-view-buttons" v-if="repositories.length">
        <div class="plitka" v-on:click="$emit('changeView')">
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
        </div>
        <div class="spisok" v-on:click="$emit('changeView')">
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
        </div>
      </div>
      <div class="repositores" v-for="(repo,i) in repositories" v-bind:key="i">
        <hr>
        <div class="repositori">
          <div class="repositori__main-info">
            <div class="add-repo-tolist" v-on:click="$emit('addRepoToMyList', repo)">
              <div class="repo-added" v-if="repo.addedStatus"></div>
            </div>
            <div class="repositori__information">
              <a v-bind:href="repo.html_url" class="repositori__header" target="_blank">
                {{ repo.full_name }}
              </a>
              <div class="repositori__info">{{ repo.description }}</div>
              <div class="tags">
                <div class="tag" v-for="(tag, i) in keyWords" v-bind:key="i"><p class="tag__text"> {{ tag }} </p></div>
              </div>
            </div>
          </div>
          <div class="repositori__stats">
            <div class="repositori__stats-stars"><svg width="25" height="23" viewBox="0 0 25 23" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M25 8.65385L16.25 7.54615L12.5 0L8.75 7.54615L0 8.65385L6.42857 14.2962L4.76786 22.5L12.5 18.4673L20.2321 22.5L18.5714 14.2962L25 8.65385Z" fill="#F1E05A"/>
                    </svg>
                    {{ starsNumber(repo.stargazers_count) }}
            </div>
            <div class="repositori__stats-language">{{ repo.language }}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="view-repo-plitka" v-if="viewStatusBar">
      <div class="change-view-buttons" v-if="repositories.length">
        <div class="plitka" v-on:click="$emit('changeView')">
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
          <div class="boxik" v-bind:class="{'boxik-active': viewStatusBar}"></div>
        </div>
        <div class="spisok" v-on:click="$emit('changeView')">
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
          <div class="spisok__line" v-bind:class="{'spisok-active': viewStatusList}"></div>
        </div>
      </div>
      <div class="repo-boxes">
        <div class="repo-box" v-for="(repo, i) of repositories" v-bind:key="i">
          <div class="repo-box__header">
            <p class="repo-box__header-language">{{ repo.language }}</p>
            <p class="repo-box__header-stars">
               <svg width="25" height="23" viewBox="0 0 25 23" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M25 8.65385L16.25 7.54615L12.5 0L8.75 7.54615L0 8.65385L6.42857 14.2962L4.76786 22.5L12.5 18.4673L20.2321 22.5L18.5714 14.2962L25 8.65385Z" fill="#F1E05A"/>
                </svg>
                {{ starsNumber(repo.stargazers_count) }}
            </p>
          </div>
          <div class="repo-box__main">
            <a class="repo-box__main-fullname" v-bind:href="repo.html_url" target="_blank">{{ repo.full_name }}</a>
            <div class="repo-box__main-description">{{ repo.description }}</div>
            <div class="repo-box__main-tags">
              <div class="tag" v-for="(tag, i) in keyWords" v-bind:key="i"><p class="tag__text">{{ tag }}</p></div>
            </div>
          </div>
          <div class="repo-box__button">
            <button class="add-repo-to-list" v-bind:class="{'delete-repo-from-list-active': repo.addedStatus}" v-on:click="$emit('addRepoToMyList', repo)"> {{ addedRepoStatus(repo) }} </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'repositoriesList',
  props: ['repositories', 'keyWords', 'viewStatusList', 'viewStatusBar'],
  methods: {
    starsNumber(stars) {
      if(stars > 1000) {
        let delStars = `${stars}`.split('');
        delStars.splice(-3);
        let resStars = delStars.join('');
        return (resStars + 'k'); 
      } else {
          return stars;
      }
    },
    addedRepoStatus(repo) {
      if (repo.addedStatus) {
        return 'REMOVE FROM LIST'
      } else {
        return 'ADD TO LIST'
      }
    }
  }
};
</script>

<style scoped lang="scss">
.repositories-list {
    width: 956px;
    margin: 0 auto;
    max-width: 100%;
}
.repositori {
    display: flex;
    justify-content: space-between;
    &__main-info {
        display: flex;
    }
    &__header {
        font-family: Roboto;
        font-style: normal;
        font-weight: bold;
        font-size: 22px;
        line-height: 26px;
        color: #0366D6;
        text-decoration: none;
    }
    &__info {
        margin-top: 4px;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        color: #000000;
    }
}
.tags {
    display: flex;
}
.tag {
    background: #F1F8FE;
    border-radius: 15px;
    margin-right: 11px;
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 14px;
    color: #000000;
    margin-top: 21px;
    &__text {
        margin: 6px 11px;
    }
}
.add-repo-tolist {
    width: 20px;
    height: 20px;
    background: #FFFFFF;
    border: 1px solid #0366D6;
    box-sizing: border-box;
    margin-top: 33px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.repositori__information {
    display: flex;
    flex-direction: column;
    margin-top: 30px;
    margin-left: 14px;
}
.repositori__stats {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    &-stars{
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 18px;
        line-height: 21px;
        color: #3C4146;
    }
    &-language {
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        color: #999999;
    }
}
.not-found-text {
    z-index: 1;
    position: absolute;
    margin-right: 50px;
    font-family: Roboto;
    font-style: normal;
    font-weight: bold;
    font-size: 36px;
    line-height: 42px;
    text-align: center;
    color: #0366D6;
}
.repositories-not-found-no-active {
    display: none;
}
.repositories-not-found {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 101px;
    &__text {
        margin-top: 30px;
        margin-bottom: 0px;
    }
    &__bottom-text {
        font-style: normal;
        font-weight: normal;
        font-size: 25px;
        line-height: 30px;
        text-align: center;
        color:black;
        margin-top: 0px;
    }
}
.plitka-disp {
    display: none;
}
.view-repo-plitka {
    width: 956px;
    margin: 0 auto;
    max-width: 100%;
}
.repo-boxes {
    width: 850px;
    margin: 0 auto;
    max-width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}
.repo-box {
    margin-top: 30px;
    width: 410px;
    background: #FFFFFF;
    border: 1px solid #E0E0E0;
    box-sizing: border-box;
    box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.25);
    padding: 24px 29px 36px 41px;
    &__header {
        display: flex;
        justify-content: space-between;
    }
    &__header-language {
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        color: #999999;
    }
    &__header-stars {
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 18px;
        line-height: 21px;
        color: #3C4146;
    }
    &__main {
        display: flex;
        flex-direction: column;
    }
    &__main-fullname {
        font-family: Roboto;
        font-style: normal;
        font-weight: bold;
        font-size: 22px;
        line-height: 26px;
        color: #0366D6;
        text-decoration: none;
    }
    &__main-description {
        margin-top: 4px;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 14.5px;
        line-height: 17px;
        color: #000000;
    }
    &__main-tags {
        display: flex;
    }
    &__button {
        margin-top: 53px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
}
.add-repo-to-list {
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
.delete-repo-from-list {
    display: none;
}
.delete-repo-from-list-active {
    display: block;
    width: 181px;
    height: 36px;
    background: #EB5757;
    box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.25);
    font-family: Roboto;
    font-style: normal;
    font-weight: normal;
    font-size: 14.5px;
    line-height: 17px;
    color: #FFFFFF;
}
.plitka, .spisok {
    width: 50px;
    height: 32px;
    margin-top: 27px;
    margin-bottom: 40px;
    cursor: pointer;
    display: flex;
    flex-wrap: wrap;
}
.change-view-buttons {
    display: flex;
    justify-content: flex-end;
    width: 970px;
    height: 70px;
}
@media (max-width: 970px) {
    .change-view-buttons-active {
        justify-content: start;
    }
}
.spisok {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: column;
    &__line {
        width: 30px;
        height: 7px;
        background: #E0E0E0;
        margin-top: 10px;
    }
}
.spisok__line:first-child {
    margin-top: 0;
}
.boxik {
    width: 14px;
    height: 14px;
    background: #E0E0E0;
    margin-left: 4px;
    margin-top: 3px;
}
.boxik:nth-child(1) {
    margin-top: 0;
}
.boxik:nth-child(2) {
    margin-top: 0;
}
.listDisplay {
    display: none;
}
.boxik-active {
    background: #3C4146;
}
.spisok-active {
    background: #3C4146;
}
.repo-no-added {
    display: none;
}
.repo-added {
    display: block;
    width: 10px;
    height: 10px;
    background: #0366D6;
    border: 1px solid #0366D6;
    box-sizing: border-box;
}
.my-repositories-list {
    display: none;
}
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
}
.not-found-text-hide {
    display: none;
}
</style>
