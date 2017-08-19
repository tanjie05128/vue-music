<template>
    <div class="recommend">
      <scroll ref="scroll" class="recommend-content" :data="discList">
        <div>
          <!--加上v-if条件判断是否recommend已经存在，若是不存在则会影响slider的mounted状态-->
          <div v-if="recommends.length" class="slider-wrapper">
            <slider>
              <div v-for="item in recommends">
                <a :href="item.linkUrl">
                  <img :src="item.picUrl" @load="loadImage" alt="">
                </a>
              </div>
            </slider>
          </div>
          <div class="recommend-list">
            <h1 class="list-title">热门歌单推荐</h1>
            <ul>
              <li v-for="item in discList" class="item">
                <div class="icon">
                  <img :src="item.imgurl" alt="" width=60 height=60>
                </div>
                <div class="text">
                  <h2 class="name" v-html="item.creator.name"></h2>
                  <p class="desc" v-html="item.dissname"></p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </scroll>
    </div>
</template>

<script type="text/ecmascript-6">
  import { getRecommend, getDiscList } from 'api/recommend'
  import { ERR_OK } from 'api/config'
  import Slider from 'base/slider/slider'
  import Scroll from 'base/scroll/scroll'

  export default {
    data() {
      return {
        recommends: [],
        discList: []
      }
    },
    created() {
      setTimeout(() => {
        this._getRecommend()
      }, 2000)
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
            this.discList = res.data.list
            console.log(this.discList)
          }
        })
      },
      loadImage() { // 绑定loadImage函数，确保在有图片撑开轮播图的时候，再去刷新scroll组件，并调用refresh方法 || 可以用这种方法防止任何后渲染出来的东西导致BScroll调用失败
        if (!this.checkLoaded) {
          this.$refs.scroll.refresh()
          this.checkLoaded = true
        }
      }
    },
    components: {
      Slider,
      Scroll
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
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
</style>
