<template>
  <div class="singer">

  </div>
</template>
<script type="text/ecmascript-6">
  import {getSingerList} from '../../api/singer'
  import {ERR_OK} from '../../api/config'
  import Singer from '../../common/js/singer'
  const HOT_NAME = '热门'
  const HOT_SINGER_LEN = 10
export default{
  data () {
    return {
      singers: []
    }
  },
  created () {
      this._getSingerList()
  },
  methods: {
    _getSingerList () {
      getSingerList().then((res)=>{
            if(res.code == ERR_OK){
              this.singers = res.data.list
              console.log(this.singers)
            }
      })
    },
    _normalizeSinger (list) {
        let map ={
          hot: {
            title: HOT_NAME,
            item: []
          }
        }
      list.forEach((item,index) => {
        if(index<HOT_SINGER_LEN){
          map.hot.items.push(new Singer({
            id: item.Fsinger_mid,
            name: item.Fsinger_name
          }))
        }
        const key = item.Findex
        if(!map[key]){
          map[key] = {
            title: key,
            item: []
          }
        }
        map[key].items.push(new Singer({
          id: item.Fsinger_mid,
          name: item.Fsinger_name,
        }))
      })
        let hot = []
        let ret = []
        for(let key in map){
          let val = map[key]
          if(val.title.match(/[a-zA-z]/)){
              ret.push(val)
          }

        }
    }
  }
}
</script>
<style scoped lang="stylus" rel="stylesheet/stylus">
    .singer
      position:fixed
      top: 88px
      bottom: 0
      width:100%
</style>
