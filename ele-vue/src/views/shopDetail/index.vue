<!--
 * @Descripttion: 商家首页
 * @version: 1.0
 * @Author: 笑佛弥勒
 * @Date: 2020-01-05 15:47:10
 * @LastEditors  : 笑佛弥勒
 * @LastEditTime : 2020-02-07 15:35:01
 -->
<template>
  <div class="shop-detail">
    <i class="go-back-icon iconfont" @click="goBack">&#xe670;</i>
    <Header :merchant-detail="merchantDetail" />
    <div class="tab-wrapper">
      <tab :tabs="tabs" />
    </div>
  </div>
</template>

<script>
// 业务组件
import Header from './components/vHeader'
import Goods from './components/goods'
import Ratings from './components/ratings'
import Seller from './components/seller'
import Tab from './components/tab'
// api请求
import { shopDetail as api } from '@/api/index'
// VUEX
import { mapMutations } from 'vuex'

export default {
  name: 'ShopDetail',
  data() {
    return {
      merchantId: this.$route.query.merchantId,
      merchantDetail: {},
      foods: [],
      tabs: [
        {
          label: '商品',
          component: Goods
        },
        {
          label: '评论',
          component: Ratings
        },
        {
          label: '商家',
          component: Seller
        }
      ]
    }
  },
  /* eslint-disable */
  components: {
    Header,
    Goods,
    Ratings,
    Seller,
    Tab
  },
  created() {
    this._getMerchantsById()
  },
  methods: {
    ...mapMutations('shopDetail', ['MERCHANTDETAIL']),
    goBack() {
      this.$router.go(-1)
    },
    // 获取店铺信息
    _getMerchantsById() {
      const params = {
        id: this.merchantId
      }
      api.getMerchantsById(params).then((res) => {
        this.merchantDetail = res.data
        this.MERCHANTDETAIL(this.merchantDetail)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.shop-detail {
  width: 100%;
  height: 100%;
  background-color: #fff;
  position: relative;
  .go-back-icon {
    position: absolute;
    z-index: 999;
    top: 5px;
    left: 0;
    padding: 5px;
    font-size: 14px;
    color:#fff;
  }
  .tab-wrapper {
    position: fixed;
    top: 136px;
    left: 0;
    right: 0;
    bottom: 0;
  }
}
</style>
