<template>
    <div class="page-wrap">
        <div class="wrap lg-show">
          <Header></Header>
        </div>
        <div class="sm-show page-sm-header-wrap">
          <smHeader></smHeader>
        </div>
        <div class="sm-show sm-search-input-wrap">
          <searchInput></searchInput>
        </div>
        <div class="main-wrap">
          <div class="wrap">
            <div class="describe-title-wrap">
              <span class="title">{{$t("navs.contractTx")}}</span>
              <ul class="link-wrap">
                <li><a href="/">{{$t("navs.home")}}</a></li>
                <li><i class="el-icon-arrow-right"></i></li>
                <li class="current">{{$t("navs.contractTx")}}</li>
              </ul>
            </div>
            <div class="des-title">
              {{$t("navs.contractAddress")}}: {{title}}
            </div>
            <el-table
              class="list-wrap"
              :empty-text="$t('message.noData')"
              :data="accountTxList"
              style="width: 100%; background: transparent">
              <el-table-column
                prop="hash"
                width="230"
                :label="$t('listHeader.hash')">
                <template slot-scope="scope">
                  <router-link :to="{path: '/transaction/detail', query: { txhash: scope.row.hash }}">
                    <span class="list-content table-link-color">{{scope.row.hash}}</span>
                  </router-link>
                </template>
              </el-table-column>
              <!-- <el-table-column
                prop="age"
                width="120"
                :label="$t('listHeader.age')">
              </el-table-column> -->
              <!-- <el-table-column
                prop="amount"
                width="100"
                :label="$t('listHeader.amount')">
              </el-table-column> -->
              <el-table-column
                prop="block"
                :label="$t('listHeader.block')"
                width="120">
              </el-table-column>
              <el-table-column
                prop="from"
                :label="$t('listHeader.from')"
                width="240">
                <template slot-scope="scope">
                  <span v-if="scope.row.inorout === true" class="list-content table-link-color" @click="toTx(scope.row.from)">{{scope.row.from}}</span>
                  <span v-else class="list-content">{{scope.row.from}}</span>
                </template>
              </el-table-column>
              <el-table-column
                prop="inorout"
                width="100"
                :label="$t('listHeader.inorout')">
                <template slot-scope="scope">
                  <span v-if="scope.row.inorout === true" class="list-content list-content-in">{{$t('tx.in')}}</span>
                  <span v-else class="list-content list-content-out">{{$t('tx.out')}}</span>
                </template>
              </el-table-column>
              <el-table-column
                prop="to"
                width="240"
                :label="$t('listHeader.to')">
                <template slot-scope="scope">
                  <span v-if="scope.row.inorout === false" class="list-content table-link-color" @click="toTx(scope.row.to)">{{scope.row.to}}</span>
                  <span v-else class="list-content">{{scope.row.to}}</span>
                </template>
              </el-table-column>
              <el-table-column
                prop="value"
                :label="$t('listHeader.value')">
              </el-table-column>
              <el-table-column
                prop="fee"
                :label="$t('listHeader.txfee')">
              </el-table-column>
            </el-table>
            <el-pagination
              class="el-pagination-wrap fr"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="page"
              :page-size="pageSize"
              layout="prev, pager, next"
              :total="total">
            </el-pagination>
          </div>
        </div>
        <Footer></Footer>
    </div>
</template>
<script>
import { mapActions } from 'vuex'
import router from '../../router'
import Header from '../header'
import smHeader from '../sm-header'
import searchInput from '../search-input'
import Footer from '../footer'

export default {
  data () {
    return {
      title: '',
      pageSize: 25
    }
  },
  components: {
    Header,
    smHeader,
    searchInput,
    Footer
  },
  mounted () {
    this.getList(1, this.$route.query)
    this.title = this.$route.query.address
  },
  computed: {
    accountTxList: {
      get () {
        return this.$store.state.account.accountTxList
      }
    },
    page: {
      get () {
        return this.$store.state.account.page
      }
    },
    total: {
      get () {
        return this.$store.state.account.total
      }
    }
  },
  methods: {
    ...mapActions(['getAccountTxList']),

    handleSizeChange (val) {
      this.getList(val, this.$route.query)
    },
    handleCurrentChange (val) {
      this.getList(val, this.$route.query)
    },
    getList (page, address) {
      this.getAccountTxList([page, address])
    },
    toTx (txHash) {
      router.push({path: '/account/detail', query: { address: txHash }})
    }
  }
}
</script>
<style lang="less">
@import "../../assets/css/page.less";
@import "../../assets/css/list.less";
@import "../../assets/css/describe.less";
</style>
