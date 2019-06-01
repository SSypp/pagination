<template lang="html">
<div>
  <div class='wynewsList'>
    <div v-for="(newsItem,index) of tempNews" :key='index' class='news_box'>
      <div class='left_img'>
        <a :href='newsItem.path'><img :src='newsItem.image' style='{width:140px;height:88px;}'></a>
      </div>
      <div class='right_info'>
        <a :href='newsItem.path' style='{color:black;}'><p class='title'>{{newsItem.title}}</p></a>
        <p class='time'>{{newsItem.passtime}}</p>
      </div>
    </div> 
  </div>
  <ul class="pagination">
    <li>
      <button type='button' @click='handleClickFirst' :disabled='isPrev'>首页</button>
    </li>
    <li>
      <button type="button" name="button" @click='handleClickPrev' :disabled='isPrev'><<</button>
    </li>
    <li v-for='(page,index) of pages' :key='index'>
      <button type="button" name="button"
      @click='handleClickPage(page.name)'
      :disabled='page.isDisabled'
      :class="{active: isActivePage(page.name)}"
      >{{page.name}}</button>
    </li>
    <li>
      <button type="button" name="button" @click='handleClickNext' :disabled='isNext'>>></button>
    </li>
    <li>
      <button type='button' @click='handleClickLast' :disabled='isNext'>尾页</button>
    </li>
  </ul>
</div>
</template>

<script>

export default {
  name: 'pagination',
  props: {
    wynews:{
      type: Array,
      required: true
    },
    maxVisibleButtons: {
      type: Number,
      required: false,
      default:3
    },
    totalPages: {
      type: Number,
      required: true
    },
    total: {
      type: Number,
      required:true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      sliceArr: []
    }
  },
  computed: {
    isPrev:function () {
      return this.currentPage === 1
    },
    isNext:function () {
      return this.currentPage === this.totalPages
    },
    startPage: function () {
      if (this.currentPage === 1) {
        return 1
      }
      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1
      }
      return this.currentPage - 1
    },
    endPage: function () {
      return Math.min(this.startPage + this.maxVisibleButtons - 1,this.totalPages)
    },
    pages: function () {
      const range = []
      for (let i = this.startPage; i <= this.endPage;i++) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage
        })
      }
      return range
    },
    tempNews: function () {
      for(let i = 0;i < this.total; i++){
        this.sliceArr.push(this.wynews[i])
      }
      if(this.total % 2 === 0) {
        return this.sliceArr.slice((this.currentPage - 1) * 5,this.currentPage * 5) 
      }
    }
  },
  methods: {
    handleClickFirst: function () {
      this.$emit('pagechanged',1)
    },
    handleClickPrev: function () {
      if(this.currentPage === 1){
        this.$emit('pagechanged',1)
      }else{
        this.$emit('pagechanged',this.currentPage - 1)
      }
    },
    handleClickPage: function (page) {
      this.$emit('pagechanged',page)
    },
    handleClickNext: function () {
      if(this.currentPage === this.totalPages){
        this.$emit('pagechanged',this.totalPages)
      }else{
        this.$emit('pagechanged',this.currentPage + 1)
      }
    },
    handleClickLast: function () {
      this.$emit('pagechanged',this.totalPages)
    },
    isActivePage: function (page) {
      return this.currentPage === page;
    }
  }
}
</script>

<style lang="css" scoped>
  ul,p{
    padding:0;
    margin:0;
  }
  a{
    text-decoration: none;
    color:black;
  }
  .wynewsList{
    width:500px;
    height:auto;
    margin:0 auto 30px;
    box-shadow: 0 0 10px #ccc;
  }
  .news_box{
    display:flex;
    padding:5px;
    background:#ddd;
    margin-bottom: 8px;
    border-radius:6px;
    height:94px;
  }
  .right_info{
    position:relative;
    width:100%;
    cursor:pointer;
  }
  .title{
    margin-left:10px;
    text-align: left;
  }
  .time{
    position:absolute;
    right:0;
    bottom: 4px;
    color:#666;
  }
  .pagination{
    display: flex;
    width: 500px;
    margin:0 auto;
  }
  .pagination li{
    margin: 0 auto;
    list-style: none;
  }
  .pagination li button{
    border:none;
    width:44px;
    height:28px;
    outline-style: none;
    font-size:14px;
  }
  .active,
  .pagination li button:hover{
    background-color: darkcyan;
    color: white;
  }
</style>
