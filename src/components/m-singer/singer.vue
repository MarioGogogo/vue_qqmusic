<template>
  <div class="singer" ref="singerRef">
    <m-listview ref="listRef" :data="singerList" @select="selectSinger"></m-listview>
    <router-view></router-view>
  </div>
</template>

<script>
import { getSingerList } from "api/singer";
import { ERROR_OK } from "api/config";
import { createSinger } from "common/js/singerClass";
import MListview from "base/listview/listview";
import { mapMutations } from "vuex";
import { playlistMixin } from 'common/js/mixin.js'
const HOT_TITLE = "热门";
const HOT_NUM   = 10;

export default {
  mixins: [playlistMixin],
  name  : "singer",
  data () {
    return {
      singerList: []
    };
  },
  created () {
    setTimeout(() => {
      this._getSingerList();
    }, 500);
  },
  methods: {
     // 当有迷你播放器时，调整滚动底部距离
    handlePlaylist(playlist) {
      let bottom = playlist.length > 0 ? '60px' : ''
      this.$refs.singerRef.style.bottom = bottom
      this.$refs.listRef.refresh()
    },
    selectSinger (item) {
      this.$router.push({
        path: `/singer/${item.id}`
      });
      this.setSinger(item);
    },
    _getSingerList () {
      getSingerList().then(res => {
        if (res.code == ERROR_OK) {
          this.singerList = this._formatSingers(res.data.list);
        }
      });
    },

    // 重组 res.data.list 数据
    _formatSingers (list) {
      let map = {
        hot: {
          title: HOT_TITLE,
          items: []
        }
      };
      //填充歌手数据
      list.forEach((item, index) => {
        if (index < HOT_NUM) {
          map.hot.items.push(createSinger(item));
        }
        let key = item.Findex;
        if (!map[key]) {
          map[key] = {
            title: key,
            items: []
          };
        }
        //填充歌手到字母数据
        map[key].items.push(createSinger(item));
      });

      //有序列表  处理map
      let hot = [];
      let ret = [];
      for (const key in map) {
        let val = map[key];
        if (val.title.match(/[a-zA-Z]/)) {
          ret.push(val);
        } else if (val.title === "热门") {
          hot.push(val);
        }
      }
      //字母排序方法
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0);
      });
      return [...hot, ...ret];
    },
    //vuex处理
    ...mapMutations({
      setSinger: "SET_SINGER"
    })
  },
  components: {
    MListview
  }
};
</script>

<style lang="less" scoped>
@import "~@/common/less/const.less";
@import "~@/common/less/mymixin.less";
.singer {
  position: fixed;
  top     : 88px;
  bottom  : 0;
  width   : 100%;
}
</style>
