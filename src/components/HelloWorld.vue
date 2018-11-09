 <template>
 <div>
  <div style="width:100%;box-sizing:border-box" :style="{display:src==''?'block':'none'}">
    <div style="float:left;width:50%;height:100vh;overflow:auto">
      <div @click="openUrl" style="margin:10px 0;border:1px solid blue;cursor:pointer">查看{{tableData[index].title}}的详细信息</div>
    <div v-for="(item,i) in tableData" @click="indexClick(i)" :key="i" :style="{width:'99%',border:'1px solid '+(i===index?'red':'black'),margin:'5px 0px',
cursor: 'pointer'}
">
      <div>{{item.title}}</div>
      <div>构建通过：{{item.build}}</div>
      <div>测试情况</div>
      <div v-for="(i,index) in item.file" :key='index' >
          {{i.url}} --->覆盖率：{{i.cover}},测试通过：{{i.coverSuccess}}
      </div>
    </div>
    </div>
      <textarea style="float:left;width:48%;height:99vh;min-height:80px">{{tableData[index].errMsg}}</textarea>
  </div>
  <div :style="{display:src===''?'none':'block',width:'100%',height:'100vh'}">
    <div @click="closeUrl" style="position:absolute;right:0;top:0;border:1px solid red;color:red;cursor:pointer">返回</div>
    <iframe style="width:100%;height:100vh" :src="src" frameborder="0"></iframe>
  </div>
  </div>
  </template>

  <script>
import axios from 'axios';
export default {
  data() {
    return {
      src: '',
      index: 0,
      tableData: [
        {
          title: 'fffffff',
          build: true,
          file: [{url: 'fffff', cover: 55, coverSuccess: false}],
          errMsg: 'dfdfdf',
        },
        {
          title: 'fffffff',
          build: true,
          file: [{url: 'fffff', cover: 55, coverSuccess: false}],
          errMsg: 'gggggg',
        },
      ],
    };
  },
  methods: {
    indexClick(index) {
      console.log(1)
      this.index = index;
    },
    getdata() {
      axios
        .get('/getdata')
        .then(item => {
          this.tableData = item.data.pr;
        })
        .catch(err => {
          console.log(err);
        });
    },
    openUrl() {
      this.src = this.tableData[this.index].prId + '/index.html';
    },
    closeUrl() {
      this.src = '';
    },
  },
  mounted() {
    this.getdata();
  },
};
</script>
<style>
</style>
