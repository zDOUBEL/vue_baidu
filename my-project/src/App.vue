<template>
  <div id="app">
     <div class="mian">
            <div class="logo">
                <img src="./assets/images/logo.png">
            </div>
            <div class="logo_input">
                <input>
                <button>百度一下</button>
            </div>
            <div class="mianWrap">
                <div class="tit">
                    <div class="shezhi">
                        <i class="icon icon_shezhi"></i>
                    </div>
                    <div class="my">
                        <i class="icon icon_my"></i>我的关注</div>
                </div>
                <div class="mainList">
                    <div class="listnav">
                        <div class="btnLeft">
                          <i class="icon icon_drop"></i>我的导航(
                        <span id="size">0</span>)
                        </div>
                        <div class="btnlist">
                            <a class="addbtn" v-on:click="clickShow">添加</a>
                            <a class="bianjibtn">编辑</a>
                            <!-- 不同的路径显示不同的切换 -->
                            <router-link to="/"  class="classicon">分类显示</router-link>
                            <router-link to="/fenlei"  class="classicon">列表显示</router-link>
                        </div>
                    </div>
                    <div class="xian" id="xian">
                      <router-view/>
                    </div>
                </div>
            </div>
        </div>
        <div class="wrap" id="wrap" v-if="isShow">
            <div class="wrapbox">
                <p class="address">添加网址</p>
                <span class="close" v-on:click="clickHide"></span>
                <div class="content_serch">
                    <div class="sug-name-wrap">
                        <input type="text" class="sug-name" placeholder="名称">
                    </div>
                    <div class="sug-link-wrap">
                        <input type="text" class="sug-link" placeholder="网址">
                    </div>
                    <div class="sug-button">添加网址</div>

                </div>
                <div class="list-conten">
                    <div class='one-class' v-for="(item, index) in items " v-bind:key="index" >
                        <div class="one-title">{{item.dirName}}</div>
                        <div class="class-recoms" v-for="(part, index) in item.items" v-bind:key="index" v-on:click="isClass(part)">
                            <div class="recom-item">
                                <span class='recom-icon' v-bind:class="{addnew:part.isadd}"></span>
                                <span class='recom-text'>{{part.name}}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      items: window.list,
      newAllList: [],
      isShow: false,
      Tabclass: false,
      classList: []
    };
  },
  methods: {
    clickShow: function() {
      this.isShow = true;
    },
    clickHide: function() {
      this.isShow = false;
      this.newData();
    },
    isClass: function(data) {
      data.isadd = !data.isadd;
      this.setHC();
    },
    chilkTab: function() {
      this.Tabclass = !this.Tabclass;
      this.classBtn();
    },
    // 处理首页数据
    newData: function() {
      var idata,
        jdata,
        newList = [],
        linshi,
        ylist = this.items;
      for (var i = 0; i < ylist.length; i++) {
        // 一级赋值，避免源数据污染
        idata = ylist[i].items;
        // console.log(list);
        linshi = JSON.parse(JSON.stringify(ylist[i])); //这里要数据处理下的JSON.parse(JSON.stringify(list[i]))
        linshi.items = [];
        for (var j = 0; j < idata.length; j++) {
          jdata = idata[j];
          if (jdata.isadd) {
            linshi.items.push(jdata);
          }
        }
        newList.push(linshi);
        // console.log(linshi)
      }
      this.newAllList = newList;
      // console.log(this.newAllList)
    },
    classBtn: function() {
      var i,
        idata,
        j,
        jdata,
        li,
        dataList = this.items,
        my = [];
      for (i = 0; i < dataList.length; i++) {
        idata = dataList[i].items;
        for (j = 0; j < idata.length; j++) {
          jdata = idata[j];
          if (jdata.isadd) {
            my.push(jdata);
            // console.log(li);
          }
        }
      }
      this.classList = my;
      console.log(this.classList);
    },
    //处理缓存
    setHC: function(val) {
      var i,
        idata,
        j,
        jdata,
        list = [],
        dataList = this.items;
      for (i = 0; i < dataList.length; i++) {
        idata = dataList[i].items;
        for (j = 0; j < idata.length; j++) {
          jdata = idata[j];
          if (jdata.isadd) {
            list.push(jdata.name);
          }
        }
      }
      // console.log(this.items)
      this.setCookie("hc", list);
      return list;
    },
    // 读取缓存,拿出缓存数据 和 源数据，循环对比，若缓存中没有源数据的名称则没有选中，反之，则选中，更改字段
    getHC: function() {
      // console.log(this.getCookie('hc'));
      var list = this.items,
        HClist,
        idata,
        jdata;
      if (this.getCookie("hc")) {
        HClist = this.getCookie("hc").split(",");
      } else {
        HClist = [];
      }
      for (var i = 0; i < list.length; i++) {
        idata = list[i].items;
        for (var j = 0; j < idata.length; j++) {
          jdata = idata[j];
          if (HClist.indexOf(jdata.name) != -1) {
            jdata.isadd = true;
          }
        }
      }
    },
    setCookie: function(name, value) {
      var Days = 30;
      var exp = new Date();
      exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000);
      document.cookie =
        name + "=" + escape(value) + ";expires=" + exp.toGMTString();
    },
    getCookie: function(name) {
      var arr,
        reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
      if ((arr = document.cookie.match(reg))) {
        return unescape(arr[2]);
      } else {
        return null;
      }
    },
    fetchData: function() {
      //纪录不同的路由路径  
      this.routePath=this.$route.path;
      console.log("this.$route", this.routePath);
    }
  },
  watch: {
    // 如果路由有变化，会再次执行该方法
    $route: "fetchData"
  },
  created: function() {
    //初始化数据 从cookies中读取  渲染到总数据
    this.getHC();
    //处理完总数据 渲染首页应该展示的数据
    this.newData();
    this.fetchData();
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.router-link-exact-active{
  color: red;
}
</style>
