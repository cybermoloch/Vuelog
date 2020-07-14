<template>
  <div class="comments">
    <vue-disqus v-if="type === 'disqus'" :shortname="config.disqusShortname"></vue-disqus>
    <vue-livere v-if="type === 'livere'" :uid="config.livereUid" :refer="path"></vue-livere>
    <Vssue v-if="type === 'vssue'" :title="nobrandtitle" :options="options"></Vssue>
  </div>
</template>

<script>
import VueDisqus from 'vue-disqus'
import VueLivere from 'vue-livere/VueLivere'

import { VssueComponent } from 'vssue'
import GithubV3 from '@vssue/api-github-v3'
import GithubV4 from '@vssue/api-github-v4'
import GitLabV4 from '@vssue/api-gitlab-v4'
import BitbucketV2 from '@vssue/api-bitbucket-v2'
import GiteeV5 from '@vssue/api-gitee-v5'
import GiteaV1 from '@vssue/api-gitea-v1'
import 'vssue/dist/vssue.css'

export default {
  name: 'vuelog-comments',

  components: {
    VueDisqus,
    VueLivere,
    Vssue: VssueComponent
  },

  props: ['path', 'nobrandtitle'],

  data () {
    // replaces string with function
    switch (this.$store.getters.config.vssueOptions.api) {
      case 'GithubV3':
        var configApi = GithubV3 // eslint-disable-line no-redeclare
        break
      case 'GithubV4':
        var configApi = GithubV4 // eslint-disable-line no-redeclare
        break
      case 'GitLabV4':
        var configApi = GitLabV4 // eslint-disable-line no-redeclare
        break
      case 'BitbucketV2':
        var configApi = BitbucketV2 // eslint-disable-line no-redeclare
        break
      case 'GiteeV5':
        var configApi = GiteeV5 // eslint-disable-line no-redeclare
        break
      case 'GiteaV1':
        var configApi = GiteaV1 // eslint-disable-line no-redeclare
        break
    }
    return {
      options: {
        api: configApi,
        owner: this.$store.getters.config.vssueOptions.owner,
        repo: this.$store.getters.config.vssueOptions.repo,
        clientId: this.$store.getters.config.vssueOptions.clientId,
        clientSecret: this.$store.getters.config.vssueOptions.clientSecret,
        baseURL: this.$store.getters.config.vssueOptions.baseURL,
        state: this.$store.getters.config.vssueOptions.state,
        labels: this.$store.getters.config.vssueOptions.lavels,
        prefix: this.$store.getters.config.vssueOptions.prefix,
        admins: this.$store.getters.config.vssueOptions.admins,
        perPage: this.$store.getters.config.vssueOptions.perPage,
        locale: this.$store.getters.lang,
        proxy: this.$store.getters.config.vssueOptions.proxy,
        issueContent: this.$store.getters.config.vssueOptions.issueContent,
        autoCreateIssue: this.$store.getters.config.vssueOptions.autoCreateIssue
      }
    }
  },

  computed: {
    config () {
      return this.$store.getters.config
    },

    type () {
      if (this.config.disqusShortname) {
        return 'disqus'
      }
      if (this.config.livereUid) {
        return 'livere'
      }
      if (this.config.vssueEnabled) {
        return 'vssue'
      }
      return null
    }
  }
}
</script>

<style lang="stylus" scoped>
  .comments
    padding-bottom 10px
</style>
