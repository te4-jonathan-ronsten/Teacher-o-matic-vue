<template>
  <div class="home">
      <Repos
        :Template="Template"
        @search-repos="onSearch"
      />
      <RepoTemplate
        v-for='repo in this.repos'
        v-show=hasSearched
        :repoCard="repo"
        :key='repo.id'
        @show-forks="showForks"
      />
      <forksLayout
        v-for='fork in this.forks'
        v-show=hasForkSearched
        :key='fork.id'
        :forkCards="fork"
      />

  </div> 
  
</template>

<script>

import Repos from "./repos.vue"
import RepoTemplate from "./showRepos.vue"
import forksLayout from "./forks.vue"
export default {
  name: "Home",
  data() {
    return {
      repos: [],
      forksData: [],
      forks: [],
      hasSearched: false,
      hasForkSearched: false
    }
  },
  components: {
    Repos,
    RepoTemplate,
    forksLayout
  },
  methods: {
    async onSearch(input){
      this.input = input
      this.repos = await this.sendRequest(`https://api.github.com/users/${this.input}/repos`)
      console.log(this.repos[0].name)
      this.hasSearched = true
    },
    async sendRequest (url) {
      const response = await fetch(url)
      const jsonResponse = await response.json()

      if (jsonResponse != null && typeof jsonResponse.message === 'string') {
        if (jsonResponse.message.includes('API rate limit exceeded')) {
          // this.errorMessage('API limit exeeded, please try again later.')
        }
      }

      return jsonResponse
    },
    async  showForks (url) {
      // clearErrorMessage()

      this.forksData = await this.sendRequest(url)
      this.hasSearched = false
      this.hasForkSearched = true


      for (const forkData of this.forksData) {
        const manifestUrl = forkData.url + '/contents/.manifest.json'
        const manifestData = await this.sendRequest(manifestUrl)


        const content = await JSON.parse(atob(manifestData.content))
        const codeFileUrl = forkData.url + '/contents/' + content.filePath
        const codeData = await this.sendRequest(codeFileUrl)

        if(codeData.message == 'Not Found') {
          codeData.content = 'File not Found'
          continue
        }

        const code = atob(codeData.content)
     
        
        const fork = {
          id: forkData.id,
          code: code,
          name: forkData.full_name, 
          htmlUrl: forkData.html_url
        }
        this.forks.push(fork)
        // const card = await createForkCard(code, forkData)
      //   // cards.push(card)
      // }

      // if (cards.length === 0) {
      //   // errorMessage('This repository has no viewable forks')
      //   return
      // }

      // const contentBox = document.querySelector('.contentBox')
      // contentBox.innerHTML = ''

      // cards.forEach(card => {
      //   contentBox.appendChild(card)
      
    
      }
    }
  }
}
</script>
