<template>
  <div class="hello">
     <div class='wo' v-for="(list,index) in newAllList"  v-bind:key="index"  v-if="list.items.length" >
         <span class='classinfo'>{{list.dirName}}</span>
         <div class='one-class'>
            <div class='lie' v-for="(part,index) in list.items"  v-bind:key="index">
                 <a>
                    <div class='lie-img'>
                        <img v-bind:src="part.imgurl">
                    </div>
                     <p>{{part.name}}</p>
                 </a>
             </div>
         </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "bar",
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
        linshi = JSON.parse(JSON.stringify(ylist[i])); //这里要数据处理下的JSON.parse(JSON.stringify(list[i]))
        linshi.items = [];
        for (var j = 0; j < idata.length; j++) {
          jdata = idata[j];
          if (jdata.isadd) {
            linshi.items.push(jdata);
          }
        }
        newList.push(linshi);
      }
      this.newAllList = newList;
      // console.log(this.newAllList)
    },
    // 读取缓存,拿出缓存数据 和 源数据，循环对比，若缓存中没有源数据的名称则没有选中，反之，则选中，更改字段
    getHC: function() {
      // console.log(this.getCookie('hc'));
      var list = this.items,HClist,idata,jdata;
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
      // console.log(this.items)
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
    this.newData();
  }
};
</script>
