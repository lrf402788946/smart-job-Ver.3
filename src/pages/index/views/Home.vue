<template lang="html">
  <div id="Home">
    <nut-navbar class="header" :leftShow="false" :rightShow="false">title</nut-navbar>
    <nut-tab>
      <nut-tab-panel tabTitle="招聘会">
        <nut-row class="selfTable" type="flex" gutter="10" align="flex-start" v-for="(item, index) in jobfairList" :key="index">
          <nut-col :span="8">
            <calendar :time="item.date"></calendar>
          </nut-col>
          <nut-col :span="8">
            <nut-row type="flex" flexWrap="reverse">
              <nut-col :span="24">{{ item.subject }}</nut-col>
              <nut-col :span="24">test</nut-col>
            </nut-row>
          </nut-col>
        </nut-row>
      </nut-tab-panel>
      <nut-tab-panel tabTitle="宣讲会">页签2</nut-tab-panel>
      <nut-tab-panel tabTitle="招聘信息">页签3</nut-tab-panel>
    </nut-tab>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex';
import methodsUtil from '@/util/methods-util';
import _ from 'lodash';
import calendar from '@/components/calendar.vue';
export default {
  name: 'Home',
  components: {
    calendar,
  },
  data() {
    return {
      //首页列表变量
      jobfairList: [],
      campusList: [],
      jobinfoList: [],
    };
  },
  async created() {
    await this.getData();
    if (_.get(this.user, 'role') === 'corp') {
      await this.getCorpInfo();
    }
  },
  computed: {
    ...mapState({
      unitList: state => state.publics.unitList,
      user: state => state.publics.user,
      corpInfo: state => state.publics.corpInfo,
      limit: state => state.self.limit,
    }),
  },
  methods: {
    ...mapActions(['loadIndexList', 'userApply', 'getCorpInfo', 'corpApply']),
    async getData() {
      const { jobfairList, campusList, jobinfoList } = await this.loadIndexList();
      console.log(jobfairList);
      this.$checkRes(jobfairList, () => {
        this.jobfairList = jobfairList.data;
      });
      this.$checkRes(campusList, () => {
        this.campusList = campusList.data;
      });
      this.$checkRes(jobinfoList, () => {
        this.jobinfoList = jobinfoList.data;
      });
    },
  },
};
</script>

<style lang="css" scoped></style>
