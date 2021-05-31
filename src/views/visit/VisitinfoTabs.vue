<template>
  <div>
    <a-tabs default-active-key="1" @change="callback">
      <a-tab-pane key="1" tab="全部">
        <Visitinfo-List ref="VisitinfoList"></Visitinfo-List>
      </a-tab-pane>
      <a-tab-pane key="2" tab="待审核" force-render>
        <Visitinfo-List ref="VisitinfoList"></Visitinfo-List>
      </a-tab-pane>
      <a-tab-pane key="3" tab="已审核">
        <Visitinfo-List ref="VisitinfoList"></Visitinfo-List>
      </a-tab-pane>
    </a-tabs>
  </div>
</template>
<script>
import VisitinfoList from './VisitinfoList'
export default {
  data() {
    return {};
  },
  components: {
    VisitinfoList
  },
  methods: {
    callback(key) {
      if(key == '1'){
        this.$refs.VisitinfoList.status = '';
        debugger;
      }else if(key == '2'){
      debugger;
      console.log(this.$refs.VisitinfoList);
        this.$refs.VisitinfoList.status = '0';
      }else{
      debugger;
        this.$refs.VisitinfoList.status = '1';
      }
    },
  },
};
</script>