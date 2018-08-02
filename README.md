# apmwebconsole
pinpoint控制台
<template>
  <div class="project-list">
    <Subtitle :title="title" :describe="describe"></Subtitle>
    <div class="filter">
      <span class="lable">项目分类：</span>
      <div class="category">
        <span class="item" v-for="item in category" :key="item.key" :type="item.key" :class="{active:item.value === curCategory}" @click="catgeoryItemClick(item)">
          {{item.value}}
        </span>
      </div>
      <hae-search class="project-search" @search="search"></hae-search>
    </div>
    <ul>
      <li v-show="!curCategory && !filter" class="add-li" @click="addClick">
        <i class="hae-icon icon-plus-circle"></i>
        <add-project-instance @addSuccess="addSuccess" @close="handleClose" v-show="isShow"></add-project-instance>
      </li>
      <li v-for="(item, index) in projectList" @click="hanleClick(item.id)" :key="index">
        <div class="left-img">
          <img :src="getImg(item.ins_icon)">
        </div>
        <div class="right-content">
          <span class="name">{{item.title }}</span>
          <span class="describe">{{item.intro}}</span>
        </div>
      </li>
    </ul>
    <div class="tip" v-show="tipMsg">{{tipMsg}}</div>
  </div>
</template>

<script>
import Subtitle from '../common/Subtitle'
import { Search, Hae, Form, FormItem, Input, Dropdown, Button } from 'hae-vue'
import AddProjectInstance from './AddProjectInstance'

export default {
  components: {
    haeSearch: Search,
    haeForm: Form,
    haeFormItem: FormItem,
    haeInput: Input,
    haeDropdown: Dropdown,
    haeButton: Button,
    Subtitle,
    AddProjectInstance
  },
  data() {
    return {
      title: '项目展示页面',
      describe:
        '基于公有云和私有云一致的云平台实现技术,希望将敏感数据放到企业内部私有部署的客户快速构建混合部署架构',
      curCategory: '',
      filter: '',
      category: [
        {
          key: 'game',
          value: '游戏'
        },
        {
          key: 'bigData',
          value: '大数据'
        },
        {
          key: 'finance',
          value: '金融'
        }
      ],
      projectList: [],
      tipMsg: '',
      isShow: false,
      show: false
    }
  },
  mounted() {
    this.getProject('', '')
  },
  computed: {},
  methods: {
    hanleClick(id) {
      window.location.href = '/#/Project-detail/' + id
    },

    catgeoryItemClick(item) {
      this.curCategory = item.value === this.curCategory ? '' : item.value
      this.getProject(this.curCategory, this.filter)
    },

    search(key, value) {
      this.filter = value
      this.getProject(this.curCategory, this.filter)
    },

    getImg(img) {
      return 'http://10.65.78.145:13456' + img
    },
    getProject(category, title) {
      Hae.ajax({
        type: 'get',
        url: 'project/?&title=' + title + '&category=' + category,
        success: data => {
          console.log(data)
          this.projectList = data
          this.tipMsg = data.length ? '' : '暂无相关数据~'
        }
      })
    },

    addClick() {
      this.isShow = true
    },

    handleClose() {
      this.isShow = false
    },

    addSuccess(data) {
      this.handleClose()
      this.projectList.unshift(data)
    }
  }
}
</script>

<style lang="less" rel="stylesheet/less" scoped>
@import './Project.less';
</style>
