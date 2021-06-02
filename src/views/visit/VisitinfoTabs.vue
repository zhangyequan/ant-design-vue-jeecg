<template>
  <div>
    <a-tabs default-active-key="2" @change="callback">
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
      status:"0",//默认打开待审核页面
    };
  },
  components: {
    VisitinfoList
  },
  methods: {
    callback(key) {
      if(key == '1'){
        this.status = '';
      }else if(key == '2'){
      this.status = '0';
      }else{
        this.status = '1';
      }
    },
  },
};
</script>