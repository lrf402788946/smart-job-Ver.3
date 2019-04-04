<template>
  <div id="app" style="margin-bottom:250px;">
    <router-view style="width:100%;" ref="router" />
    <footBar ref="tabbar"></footBar>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import footBar from '@/components/footBar.vue';
export default {
  name: 'App',
  metaInfo: {
    title: '',
    titleTemplate: '智慧就业 - %s',
  },
  components: {
    footBar,
  },
  async created() {
    await this.getUser();
    await this.loadUnit();
  },
  methods: {
    //获取用户信息,获取分站列表
    ...mapActions(['getUser', 'loadUnit']),
  },
};
</script>

<style lang="less"></style>
