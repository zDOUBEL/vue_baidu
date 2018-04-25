<template>
   <div class="hello">
      <div>
          <div class="list_class">
              <div >
                  <div class='lie_info' v-for="(classinfo,index) in classList"  v-bind:key="index">
                      <a>
                          <div class='lie-img_info'>
                              <img v-bind:src="classinfo.imgurl" alt="">
                          </div>
                          <p>{{classinfo.name}}</p>
                      </a>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>
<script>
export default {
  name: "foo",
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
          }
        }
      }
      this.classList = my;
    },
    // 读取缓存,拿出缓存数据 和 源数据，循环对比，若缓存中没有源数据的名称则没有选中，反之，则选中，更改字段
    getHC: function() {
      var list = this.items,
        HClist,
        idata,
        jdata;
      if (this.getCookie("hc") != "") {
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
    getCookie: function(name) {
      var arr,
        reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
      if ((arr = document.cookie.match(reg))) {
        return unescape(arr[2]);
      } else {
        return null;
      }
    }
  },
  created: function() {
    //初始化数据 从cookies中读取  渲染到总数据
    this.getHC();
    //处理完总数据 渲染首页应该展示的数据
    this.classBtn();
  }
};
</script>
