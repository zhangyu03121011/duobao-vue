<template>
  <div class="buy-list-page page">
    <quick-link :linksArray="['personal', 'time', 'home']" />
    <detail-cmp :open="detailPopupStatus" @closePopup="closePopup" :details="currentDetails" />
    <div :class="`list-container ${detailPopupStatus ? 'blur': ''}`">
      <div class="empty-tips" v-if="noRecord">暂无更多数据</div>
      <div class="item" v-for="item in list" :key="item.id">
        <div class="info-part">
          <div class="pic-part">
            <img :src="item.picSrc" class="pic"/>
            <div :class="`label ${item.type}`"></div>
          </div>
          <div class="detail-part">
            <div>{{item.title}}</div>
            <div><span>本期参与: </span><span class="yellow-cl">{{item.canyu}}</span></div>
            <div><span>参与时间: </span><span>{{`${item.takePartDate} ${item.takePartTime}`}}</span></div>
            <div class="red-cl">
              <span>参与段号: </span>
              <span>{{`${item.duanHao.start}-${item.duanHao.end}`}}</span>
              <span>{{` X ${item.num}`}}</span>
            </div>
            <div v-if="item.type !== 'waiting'">
              <span>开奖时间: </span>
              <span>{{`${item.openDate} ${item.openTime}`}}</span>
            </div>
            <div v-if="item.type !== 'waiting'"><span>获胜号码: </span><span class="red-cl">{{item.winNumber}}</span></div>
          </div>
        </div>
        <div class="action-part"><div class="btn" @click="openDetail(item.id)">查看详情</div></div>
      </div>
    </div>
  </div>
</template>

<script>
import detailCmp from './battle-details'
import quickLink from '@/components/quick-link'
import utils from '@/lib/utils'
import api from './api'

export default {
  components: {detailCmp, quickLink},
  computed: {
    noRecord: function () {
      return !this.list || this.list.length === 0
    }
  },
  beforeMount () {
    this.list = api.getList()
  },
  data () {
    return {
      list: [],
      currentDetails: {},
      detailPopupStatus: false
    }
  },
  methods: {
    openDetail: function (id) {
      this.currentDetails = api.getDetails(id)
      this.detailPopupStatus = true
      utils.dom.bodyDisableScroll()
    },
    closePopup: function () {
      this.detailPopupStatus = false
      utils.dom.bodyEnableScroll()
    }
  }
}
</script>
