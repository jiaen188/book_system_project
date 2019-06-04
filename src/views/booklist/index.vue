<template>
  <div class="app-container">
    <div class="filter-container" style="margin-bottom:20px">
      <el-input placeholder='请输入书名' v-model="queryName" style="width: 200px;" class="filter-item"/>

      <el-input placeholder="请输入ISBN" v-model="queryNumber" style="width: 200px;" class="filter-item"/>

      <el-select v-model="queryStatus" clearable style="width: 90px" class="filter-item">
        <el-option v-for="item in statusOptions" :key="item" :label="item" :value="item"/>
      </el-select>

      <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">查询</el-button>
    </div>

      <el-table
        :key="id"
        :data="list"
        border
        fit
        highlight-current-row
        style="width: 100%;">
        <el-table-column label="书名" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.title }}</span>
          </template>
        </el-table-column>
        <el-table-column label="ISBN" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.isbn13 }}</span>
          </template>
        </el-table-column>
        <el-table-column label="状态" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.statusText }}</span>
          </template>
        </el-table-column>
        <el-table-column label="定金" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.price }}</span>
          </template>
        </el-table-column>
        <el-table-column label="最近更新时间" align="center">
          <template slot-scope="scope">
            <span>{{ scope.row.apply_at }}</span>
          </template>
        </el-table-column>
        <el-table-column label="拥有者" align="center">
          <template slot-scope="scope">
            <span>{{ scope.row.ownerName }}</span>
          </template>
        </el-table-column>
      </el-table>


  </div>
</template>

<script>
import axios from 'axios'

const statusList = {
  100: '未入库',
  101: '待采购',
  102: '采购中',
  201: '在库',
  202: '借出',
  301: '损毁',
  302: '遗失'
}

export default {
  data() {
    return {
      token: '',
      queryName: '',
      queryNumber: '',
      queryStatus: '全部',
      statusOptions: [
        '全部',
        '带采购',
        '采购中',
        '入库',
        '借出',
        '注销'
      ],
      list: []
    }
  },
  mounted() {
    this.getToken()
    // this.getMyApply()
  },
  methods: {
    handleFilter() {
      console.log('query')
    },
    getToken() {
      this.$http.get('http://hm2.hwd.cn/api/v1/auth?username=淡漠').then(res => {
        console.log('http*********', res)
        if (res.body.code === 0) {
          this.token = res.body.data.token
          localStorage.setItem('token', this.token)
          this.getMyApply()
        }
      })
    },
    getMyApply() {
      this.$http({
        method: 'get',
        url: 'http://hm2.hwd.cn/api/v1/admin/books',
        headers: {
          'authorization': `bearer ${this.token}`
        }
      }).then(res => {
        console.log('******myapply', res)
        this.list = res.body.data.data.map(item => {
          return {
            ...item,
            statusText: statusList[item.status]
          }
        })
      })
    }
  },
  components: {

  }
}
</script>

<style lang="scss" scoped>

</style>
