<template lang="html">
  <div id="Home">
    <nut-navbar class="header" :leftShow="false" :rightShow="false">title</nut-navbar>
    <nut-swiper :paginationVisible="true" direction="horizontal" ref="demo1">
      <img class="nut-swiper-slide" :src="imgs" />
    </nut-swiper>
    <!-- <nut-tab>
      <nut-tab-panel tabTitle="招聘会">
        <nut-row class="selfTable" type="flex" :gutter="2" align="flex-start" v-for="(item, index) in jobfairList" :key="index">
          <nut-col :span="7">
            <calendar :time="item.date"></calendar>
          </nut-col>
          <nut-col :span="15">
            <nut-row type="flex" flexWrap="reverse">
              <nut-col :span="24" class="tableContent"> 地址:{{ item.address }} </nut-col>
              <nut-col :span="24" class="tableContent">类型:{{ item.type }}</nut-col>
              <nut-col :span="24" class="tableContent">时间:{{ item.time }}</nut-col>
              <nut-col :span="24" class="tableTitle">{{ item.subject }}</nut-col>
            </nut-row>
          </nut-col>
          <nut-col :span="2">
            <el-button type="success" icon="el-icon-plus" circle></el-button>
          </nut-col>
        </nut-row>
        <nut-row class="selfTable" type="flex" :gutter="2" align="flex-start" v-for="(item, index) in jobfairList" :key="index">
          <nut-col :span="7">
            <calendar :time="item.date"></calendar>
          </nut-col>
          <nut-col :span="15">
            <nut-row type="flex" flexWrap="reverse">
              <nut-col :span="24" class="tableContent"> 地址:{{ item.address }} </nut-col>
              <nut-col :span="24" class="tableContent">类型:{{ item.type }}</nut-col>
              <nut-col :span="24" class="tableContent">时间:{{ item.time }}</nut-col>
              <nut-col :span="24" class="tableTitle">{{ item.subject }}</nut-col>
            </nut-row>
          </nut-col>
          <nut-col :span="2">
            <el-button type="success" icon="el-icon-plus" circle></el-button>
          </nut-col>
        </nut-row>
        <nut-row class="selfTable" type="flex" :gutter="2" align="flex-start" v-for="(item, index) in jobfairList" :key="index">
          <nut-col :span="7">
            <calendar :time="item.date"></calendar>
          </nut-col>
          <nut-col :span="15">
            <nut-row type="flex" flexWrap="reverse">
              <nut-col :span="24" class="tableContent"> 地址:{{ item.address }} </nut-col>
              <nut-col :span="24" class="tableContent">类型:{{ item.type }}</nut-col>
              <nut-col :span="24" class="tableContent">时间:{{ item.time }}</nut-col>
              <nut-col :span="24" class="tableTitle">{{ item.subject }}</nut-col>
            </nut-row>
          </nut-col>
          <nut-col :span="2">
            <el-button type="success" icon="el-icon-plus" circle></el-button>
          </nut-col>
        </nut-row>
        <nut-row class="selfTable" type="flex" :gutter="2" align="flex-start" v-for="(item, index) in jobfairList" :key="index">
          <nut-col :span="7">
            <calendar :time="item.date"></calendar>
          </nut-col>
          <nut-col :span="15">
            <nut-row type="flex" flexWrap="reverse">
              <nut-col :span="24" class="tableContent"> 地址:{{ item.address }} </nut-col>
              <nut-col :span="24" class="tableContent">类型:{{ item.type }}</nut-col>
              <nut-col :span="24" class="tableContent">时间:{{ item.time }}</nut-col>
              <nut-col :span="24" class="tableTitle">{{ item.subject }}</nut-col>
            </nut-row>
          </nut-col>
          <nut-col :span="2">
            <el-button type="success" icon="el-icon-plus" circle></el-button>
          </nut-col>
        </nut-row>
      </nut-tab-panel>
      <nut-tab-panel tabTitle="宣讲会">页签2</nut-tab-panel>
      <nut-tab-panel tabTitle="招聘信息">页签3</nut-tab-panel>
    </nut-tab> -->

    <el-tabs v-model="activeName">
      <el-tab-pane label="用户管理" name="first">用户管理<br/>用户管理<br/>用户管理<br/>用户管理<br/>用户管理<br/>用户管理<br/>用户管理<br/>用户管理<br/></el-tab-pane>
      <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
      <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
      <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
    </el-tabs>
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
    // calendar,
  },
  data() {
    return {
      activeName: 'first',
      imgs: require('@/assets/img/push_3.jpg'),
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
