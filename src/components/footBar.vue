<template lang="html">
  <div id="footBar">
    <nut-tabbar :tabbarList="menuList" :bottom="true"> </nut-tabbar>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import _ from 'lodash';
export default {
  name: 'footBar',
  components: {},
  data() {
    return {
      actIndexIcon: require('../assets/img/首页1.png'),
      indexIcon: require('../assets/img/首页.png'),
      actMyIcon: require('../assets/img/我的1.png'),
      myIcon: require('../assets/img/我的.png'),
      actAccount: require('../assets/img/账号密码1.png'),
      account: require('../assets/img/账号密码.png'),
    };
  },
  computed: {
    ...mapState({
      user: state => state.publics.user,
    }),
    selected: {
      get() {
        let uri = window.location.pathname;
        return uri;
      },
    },
    myUrl: {
      get() {
        let role = _.get(this.user, 'role');
        if (role === 'corp') {
          return 'corp.html';
        } else if (role === 'user') {
          return 'user.html';
        } else {
          return 'guest.html';
        }
      },
    },
    menuList: {
      get() {
        let list = [
          { tabTitle: '首页', icon: this.indexIcon, activeIcon: this.actIndexIcon, curr: false, href: 'index.html' },
          { tabTitle: '我的', icon: this.myIcon, activeIcon: this.actMyIcon, curr: false, href: '' },
          { tabTitle: '账号', icon: this.account, activeIcon: this.actAccount, curr: false, href: '/weixin/ui/center/index.html' },
        ];
        if (this.selected.includes('index.html')) list[0]['curr'] = true;
        else if (this.selected.includes(this.myUrl)) list[1]['curr'] = true;
        list[1]['href'] = this.myUrl;
        return list;
      },
    },
  },
  methods: {},
};
</script>

<style lang="css" scoped></style>
