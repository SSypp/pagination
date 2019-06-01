<template>
  <div class="hello">
    <Pagination :wynews='wynews' :totalPages='Math.ceil(newsTotal / 5)' :total='newsTotal' :current-page='currentPage' @pagechanged='handleChange'/>
  </div>
</template>

<script>
import Pagination from './pagination'
export default {
  name: 'HelloWorld',
  components: {
    Pagination
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      currentPage: 1,
      wynews: [],
      newsTotal: 0,
    }
  },
  methods:  {
      handleChange: function (page) {
        this.currentPage = page;
      }
  },
  mounted () {
    this.$axios.get('/api/wynews.json')
    .then(
      res => {
      //console.log(res.data.result)
      this.wynews = res.data.result
      this.newsTotal = res.data.result.length
      //console.log(this.wynews)
      //console.log(this.newsTotal)
    })
    .catch(
      err => {
        console.log(err)
      }
    )
  }
  //   this.$axios({
  //     method:'post',
  //     url:'https://api.apiopen.top/getWangYiNews',
  //     data:{}
  //     }).then((res) =>{          //这里使用了ES6的语法
  //       console.log(res.data)
  //       this.wynews = res.data.result      //请求成功返回的数据
  //       this.newsTotal = res.data.result.length
  //     }).catch((error) =>{
  //       console.log(error)       //请求失败返回的数据
  //     })
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
