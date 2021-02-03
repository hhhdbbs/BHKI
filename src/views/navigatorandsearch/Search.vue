<template>

  <div>
    <link rel="stylesheet" href="//at.alicdn.com/t/font_2289420_o8h2g96gm3i.css">
    <new-navigation></new-navigation>
    <el-container>
      <el-main style="text-align: center;">
        <div style="height: 280px;">
          <img src="../../assets/logoT.png" height="320" width="350"/></div>
        <div style="margin-top: 15px; margin-bottom: 90px;" id="searchInput">
          <el-input placeholder="请输入内容" v-model="input3" style="width: 900px;">
            <el-select v-model="value" slot="prepend" placeholder="标题" style="width: 140px;">
              <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                <div @click="getType(item.label)">{{item.label}}</div>
              </el-option>
            </el-select>
            <el-button slot="append" icon="el-icon-search" @click="sendSearch(input3, type)"></el-button>
          </el-input>
          <el-link type="danger" :underline="false" style="margin-left: 10px; font-size: 16px"
                   @click="gotoCompSearch()">高级检索
          </el-link>
        </div>
        <div style="margin: 60px auto;display: flex;justify-content: space-evenly;">
          <!--                卡片-1-左-->
          <div >
            <div>
              <el-card shadow="hover" style="width: 380px;height: 520px;" class="box-card" >
                <div slot="header" class="clearfix">
                  <span style="float: left">学术焦点</span>
                </div>
                <div>
                </div>
                <div style="width: 100%;height: 240px;">
                  <div v-for="rank in ranks"  style="width: 100%; margin-top: 3%; display: flex;justify-content: center;padding-bottom: 10px;border-bottom: #dedddc 1px solid">
                    <div class="iconfont icon-icon_jiaodian" style="font-size: 23px;font-weight: bold;margin-right: 10px;color: #ffa925"></div>
                    <div style="width: 100%; margin-top: 7px;line-height: 20px;text-align: left">
                      <el-link v-bind:href="'/paper/?paper_id='+rank.id" :underline="false" style="=display: inline-block;
                                                                                                                        overflow: hidden;
                                                                                                                        text-overflow: ellipsis;
                                                                                                                        display: -webkit-box;
                                                                                                                        -webkit-line-clamp: 1;
                                                                                                                        -webkit-box-orient: vertical;">{{rank.title}}
                      </el-link>
                    </div>
                  </div>
                </div>
              </el-card >
            </div>
          </div>
          <!--                卡片-1-中-->
          <div>
            <div>
              <el-card shadow="hover" style="width: 380px;height: 520px;" class="box-card" >
                <div slot="header" class="clearfix">
                  <span style="float: left">热门专家</span>
                </div>
                <div style="width: 100%; height: 240px">
                  <div v-for="lib in library" style="width: 100%; margin-top: 3%; display: flex;justify-content: center;padding-bottom: 10px;border-bottom: #dedddc 1px solid">
                    <div class="iconfont icon-zhuanjia2" style="font-size: 23px;font-weight: bold;margin-right: 10px;color: #ffa925"></div>
                    <!--                                        <svg class="icon"-->
                    <!--                                             style="float: left;margin-top: 5px;margin-left: 5px; font-size: 30px">-->
                    <!--                                            <use xlink:href="#icon-tangguo"></use>-->
                    <!--                                        </svg>-->
                    <div style=" width: 100%; margin-top: 7px;line-height: 20px;text-align: left">
                      <el-link v-bind:href="'/author?author_id='+lib.id" :underline="false" style="display: inline-block;
                                                                                                                        overflow: hidden;
                                                                                                                        text-overflow: ellipsis;
                                                                                                                        display: -webkit-box;
                                                                                                                        -webkit-line-clamp: 1;
                                                                                                                        -webkit-box-orient: vertical;">{{lib.name}}</el-link>
                    </div>
                  </div>
                </div>
              </el-card>
            </div>
          </div>
          <!--                卡片-1-右-->
          <div>
            <div>
              <el-card shadow="hover" style="width: 380px;height: 520px;" class="box-card" >
                <div slot="header" class="clearfix">
                  <span style="float: left">热门帖子</span>
                </div>
                <div v-for="sci in science" class="text item"
                     style="width: 100%; margin-top: 3%;display: flex;justify-content: center;padding-bottom: 10px;border-bottom: #dedddc 1px solid">
                  <div class="iconfont icon-971caidan_xinjiantiezi" style="font-size: 23px;font-weight: bold;margin-right: 10px;color: #ffa925"></div>
                  <!--                                    <svg class="icon"-->
                  <!--                                         style="float: left;width: 40px;height: 35px; margin: -2px 0px 15px 0px;">-->
                  <!--                                        <use xlink:href="#icon-tianshi"></use>-->
                  <!--                                    </svg>-->
                  <div style="width: 100%;text-align: left;margin-top: 7px;line-height: 20px;">
                    <el-link v-bind:href="'/BlogItem/'+sci.userid+'/'+sci.blogid" :underline="false" style="display: inline-block;
                                                                                                                        overflow: hidden;
                                                                                                                        text-overflow: ellipsis;
                                                                                                                        display: -webkit-box;
                                                                                                                        -webkit-line-clamp: 1;
                                                                                                                        -webkit-box-orient: vertical">{{sci.blogname}}
                    </el-link>
                  </div>
                </div>

              </el-card>
            </div>
            <!--                      <el-divider></el-divider>-->
          </div>
        </div>
      </el-main>
      <el-footer>

      </el-footer>
    </el-container>
  </div>

</template>

<script>
import NewNavigation from '../navigatorandsearch/NewNavigation';

export default {
  name: "Search",
  components: {
    NewNavigation
  },
  data() {
    return {
      professor: '',
      proRank: '#1',
      proIntroduction: 'Pro Introduction',
      options: [{
        value: '选项1',
        label: '标题'
      }, {
        value: '选项2',
        label: '关键词'
      }, {
        value: '选项3',
        label: '作者'
      }, {
        value: '选项4',
        label: '机构'
      }, {
        value: '选项5',
        label: '领域'
      }, {
        value: '选项6',
        label: '刊物'
      }, {
        value: '选项7',
        label: '摘要'
      }, {
        value: '选项8',
        label: 'ISSN'
      }, {
        value: '选项9',
        label: 'ISBN'
      }, {
        value: '选项10',
        label: 'DOI'
      }],
      value: '',
      ranks: [{
        id: '',
        title: '',
        n_citation: ''
      }],
      library: [{
        id: '',
        name: '',
        h_index: '',
        n_pubs: '',
        n_citation: '',
      }],
      science: [{
        avatar: '',
        blogid: '',
        blogname: '',
        htmlcontent: '',
        likenum: '',
        readnum: '',
        textcontent: '',
        tipnum: '',
        userid: '',
      }],
      input3: '',
      type: '标题'
    }
  },
  mounted() {
    this.getRanks()
    this.getLibrary()
    this.getScience()
  },
  methods: {
    //高级检索跳转
    gotoCompSearch() {
      this.$router.push('/AdvancedSearch')
    },
    //学术焦点
    getRanks() {
      var that = this
      this.$axios.post('/apis/search/popularpapers',
      ).then(res => {
        console.log(res);
        that.ranks = res.data
      })
    },
    //热门专家
    getLibrary() {
      var that = this
      this.$axios.post('/apis/search/popularauthors')
          .then(res => {
            console.log(res);
            that.library = res.data
          })
    },
    // 热门帖子
    getScience() {
      var that = this
      this.$axios.post('/apis/blog/gethotblogs',
          {
            type: 0
          },)
          .then(res => {
            console.log(res);
            that.science = res.data.data.list
          })
    },
    getType(label) {
      this.type = label
    },
    sendSearch(input, type) {
      if (input.length === 0) {
        this.$message({
          type: 'info',
          message: '请输入搜索内容！'
        })
      } else {
        this.$router.push({
          path: '/Searching',
          query: {
            input: input,
            type: type
          }
        })
      }
    }
  }
}
</script>

<style>
body {
  background-image: url('../../assets/image/user/image/login-back.png');
}
</style>
<style lang="scss" scoped>
#searchInput{
  /deep/ .el-input-group__prepend{
    background-color:#FFF;
    border:2px solid #df5747;
    box-shadow: 0 1px 4px 0 rgba(0,0,0,.2);
    border-right-width: 0;
  }
  /deep/ .el-input-group>.el-input__inner{
    border:2px solid #df5747;
    box-shadow: 0 1px 4px 0 rgba(0,0,0,.2);
    border-right-width: 0;
    border-left-width: 0;
  }
  /deep/ .el-input-group__append{
    background-color:#FFF;
    border:2px solid #df5747;
    box-shadow: 0 1px 4px 0 rgba(0,0,0,.2);
    border-left-width: 0;
  }
}

/deep/ .el-carousel__item[data-v-1a1d373c]:nth-child(2n+1) {
  background-color: #df5747;
  color: white;
}

/deep/ .el-carousel__item[data-v-1a1d373c]:nth-child(2n) {
  background-color: #f9ae8f;
  color: white;
}

/deep/ .el-carousel__mask {
  width: 100%;
  background-color: #FFF;
  opacity: .24;
  transition: .2s;
}

/*#df5747*/
/deep/ .el-card__header {
  background-color: #df5747;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}

/deep/ .el-card__body {
  display: flex;
  flex-direction: column;
  background-color: white;
  padding-top: 16px;
}

/deep/ .el-input__inner {
  height: 55px;
  border-style: solid solid solid solid;
  font-size: 16px;
}

/deep/ .el-input-group__prepend {
  /*background-color: #db8fbe;*/
  border-style: solid;
  border-radius: 27px 0 0 27px;
  /*color: white;*/
}

/deep/ .el-input-group__append {
  /*background-color: #db8fbe;*/
  border-style: solid;
  border-radius: 0 27px 27px 0;
  /*color: white;*/
}

.el-select .el-input {
  width: 130px;
}

.input-with-select .el-input-group__prepend {
  background-color: #fff;
}

.text {
  font-size: 14px;
}

.item {
  /*margin-bottom: 18px;*/
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both
}

.box-card {
  width: 480px;
  border-radius: 10px;
  border: none;
}

.el-carousel__item h3 {
  color: mediumpurple;
  font-size: 14px;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
</style>
