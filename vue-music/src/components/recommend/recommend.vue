<template>
    <div class="recommend">
      <div class="recommend-content">
        <!--加上v-if条件判断是否recommend已经存在，若是不存在则会影响slider的mounted状态-->
        <div v-if="recommends.length" class="slider-wrapper">
          <slider>
            <div v-for="item in recommends">
              <a :href="item.linkUrl">
                <img :src="item.picUrl" alt="">
              </a>
            </div>
          </slider>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul></ul>
        </div>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
  import { getRecommend, getDiscList } from 'api/recommend'
  import { ERR_OK } from 'api/config'
  import Slider from 'base/slider/slider'

  export default {
    data() {
      return {
        recommends: []
      }
    },
    created() {
      this._getRecommend()
      this._getDiscList()
    },
    methods: {
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
//            console.log(this.recommends)
          }
        })
      },
      _getDiscList() {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            console.log(res.data)
          }
        })
      }
    },
    components: {
      Slider
    }
  }
</script>

<style lang="stylus" type="text/stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl"

  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
</style>
