<template>
  <ul class="navigater" oncontextmenu="return false" onselectstart="return false">
    <li
      :key="item.id"
      v-for="item of items">
      <el-tooltip class="item" effect="light" placement="right"
        v-if="item.child">
        <div class="navigater-lnk" slot="content">
          <a
            :key="it.id"
            :class="it.style"
            @click.stop="lnkChange(item, it)"
            v-html="it.name"
            v-for="it of item.child"
            v-if="it.name !== '老师管理' || (it.name === '老师管理' && isadmin === '管理员')"></a>
        </div>
        <el-button
          :class="item.id + ' ' + item.style"
          @click="navChange(item)">{{item.name}}</el-button>
      </el-tooltip>
      <el-button
        :class="item.id + ' ' + item.style"
        @click="navChange(item)"
        v-else>{{item.name}}</el-button>
    </li>
  </ul>
</template>

<script>
export default {
  components: {
  },
  props: [
  ],
  data () {
    return {
      // 控制器数据列表
      items: [
        {
          id: 'nav1',
          name: '工作台',
          type: '_pathTo',
          path: 'Home',
          style: 'active'
        },
        {
          id: 'nav2',
          name: '资源中心',
          type: '_hrefTo',
          path: 'lib/gather/',
          style: '',
          child: [
            { name: '试题库', type: '_hrefTo', path: 'lib/gather/', style: '', default: true },
            { name: '制题', type: '_hrefTo', path: 'question/edit/', style: '' },
            { name: '试卷库', type: '_hrefTo', path: 'paper/gather/', style: '' },
            { name: '制卷', type: '_hrefTo', path: 'paper/add/', style: '' }
          ]
        },
        {
          id: 'nav3',
          name: '教学功能',
          type: '_hrefTo',
          path: 'homeworks/checking/',
          style: '',
          child: [
            { name: '考试记录', type: '_hrefTo', path: 'data/', style: '', default: true },
            { name: '统考/抽考', type: '_hrefTo', path: 'paper/unified_exam/', style: '' },
            { name: '学生考试数据', type: '_hrefTo', path: 'data/student/', style: '' },
            { name: '练习库', type: '_hrefTo', path: 'homeworks/', style: '' },
            { name: '练习记录', type: '_hrefTo', path: 'homeworks/checking/', style: '' }
          ]
        },
        {
          id: 'nav4',
          name: '教务管理',
          type: '_pathTo',
          path: 'StudentAdmin',
          style: '',
          child: [
            { name: '老师管理', type: '_pathTo', path: 'TeacherAdmin', style: '' },
            { name: '学生管理', type: '_pathTo', path: 'StudentAdmin', style: '', default: true },
            { name: '班级管理', type: '_pathTo', path: 'ClassroomAdmin', style: '' },
            { name: '考勤管理', type: '_pathTo', path: 'KaoqinAdmin', style: '' }
          ]
        },
        {
          id: 'nav5',
          name: '活动档案',
          type: '_pathTo',
          path: 'GroupAdmin',
          style: '',
          child: [
            { name: '社团小组', type: '_pathTo', path: 'GroupAdmin', style: '', default: true },
            { name: '艺术实践', type: '_pathTo', path: 'ActivityAdmin', style: '' },
            { name: '特长认证', type: '_pathTo', path: 'SkillAdmin', style: '' }
          ]
        },
        {
          id: 'nav6',
          name: '过程性评价',
          type: '_pathTo',
          path: 'ProcessAdmin',
          style: ''
        }
      ],

      isadmin: window.Global.database.userPower // 是否管理角色
    }
  },
  watch: {
    $route (to) {
      console.log(1, to)
    }
  },
  computed: {
  },
  methods: {
    /**
     * [initial 初始化组件]
     * @return {[]} []
     */
    initial () {
      // console.log(0, this.$route)
    },

    /**
     * [navChange 导航切换]
     * @param  {[Object]} item [元素数据]
     * @return {[]} []
     */
    navChange (item) {
      this.clearStyle()
      this.$emit(item.type, item.path)

      item.style = 'active'
      if (item.child) {
        for (let it of item.child) {
          if (it.default) {
            it.style = 'active'
          }
        }
      }
    },

    /**
     * [lnkChange 连接切换]
     * @param  {[Object]} item [元素数据]
     * @param  {[Object]} it [子元素数据]
     * @return {[]} []
     */
    lnkChange (item, it) {
      this.clearStyle()
      this.$emit(it.type, it.path)

      item.style = 'active'
      it.style = 'active'
    },

    /**
     * [clearStyle 清除样式]
     * @return {[]} []
     */
    clearStyle () {
      for (let item of this.items) {
        item.style = ''
        if (item.child) {
          for (let it of item.child) {
            it.style = ''
          }
        }
      }
    }
  },
  created () {
    this.initial()
  },
  mounted () {
  }
}
</script>
