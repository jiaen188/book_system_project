<template>
  <div class="app-container">

      <div class="title">
          年度书籍采购统计
      </div>
      <el-table
        :key="id"
        :data="yearList"
        border
        fit
        highlight-current-row
        style="width: 100%;">
        <el-table-column label="年度" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.year }}</span>
          </template>
        </el-table-column>
        <el-table-column label="采购书籍量" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.num }}</span>
          </template>
        </el-table-column>
        <el-table-column label="采购金额" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.momeny }}</span>
          </template>
        </el-table-column>
      </el-table>

        <div class="title">
          季度书籍采购统计
      </div>
      <el-table
        :key="id"
        :data="seasonList"
        border
        fit
        highlight-current-row
        style="width: 100%;">
        <el-table-column label="季度" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.name }}</span>
          </template>
        </el-table-column>
        <el-table-column label="采购书籍量" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.num }}</span>
          </template>
        </el-table-column>
        <el-table-column label="采购金额" align="center" >
          <template slot-scope="scope">
            <span>{{ scope.row.momeny }}</span>
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
        '采购中'
      ],
      list: [],
      yearList: [
        {
            id: 1,
            year: '2018年',
            num: '103',
            momeny: '2034'
        },
        {
            id: 2,
            year: '2017年',
            num: '163',
            momeny: '2634'
        },
        {
            id: 3,
            year: '2016年',
            num: '123',
            momeny: '2023'
        }
      ],
      seasonList: [
         {
            id: 1,
            name: '第一季度',
            num: '23',
            momeny: '453'
        },
        {
            id: 2,
            name: '第二季度',
            num: '43',
            momeny: '634'
        },
        {
            id: 3,
            name: '第三季度',
            num: '13',
            momeny: '223'
        },
        {
            id: 4,
            name: '第四季度',
            num: '33',
            momeny: '435'
        } 
      ]
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
          this.getStatistics()
        }
      })
    },
    getStatistics() {
      this.$http({
        method: 'get',
        url: 'http://hm2.hwd.cn/api/v1/admin/statistics',
        headers: {
          'authorization': `bearer ${this.token}`
        }
      }).then(res => {
        console.log('******/statistics', res)
        
      })
    }
  },
  components: {

  }
}
</script>

<style lang="scss" scoped>
.title {
    margin-top: 30px;
    margin-bottom: 20px;
}
</style>
