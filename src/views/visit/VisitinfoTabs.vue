<template>
  <div>
    <a-tabs default-active-key="1" @change="callback">
      <a-tab-pane key="1" tab="全部">
        <VisitinfoList :status="status"></VisitinfoList>
      </a-tab-pane>
      <a-tab-pane key="2" tab="待审核">
        <VisitinfoList :status="status"></VisitinfoList>
      </a-tab-pane>
      <a-tab-pane key="3" tab="已审核">
        <VisitinfoList :status="status"></VisitinfoList>
      </a-tab-pane>
    </a-tabs>
  </div>
</template>
<script>
import VisitinfoList from './VisitinfoList'
export default {
  data() {
    return {
      status:"",
    };
  },
  components: {
    VisitinfoList
  },
  methods: {
    callback(key) {
      if(key == '1'){
        this.status = '';
        debugger;
      }else if(key == '2'){
      debugger;
      this.status = '0';
      }else{
      debugger;
        this.status = '1';
      }
    },
  },
};
</script>