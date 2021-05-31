<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('visitinfo')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <visitinfo-modal ref="modalForm" @ok="modalFormOk"></visitinfo-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import VisitinfoModal from './modules/VisitinfoModal'
  import {getAction} from '@/api/manage'

  export default {
    name: 'VisitinfoList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      VisitinfoModal
    },
    data () {
      return {
        description: 'visitinfo管理页面',
        status:'',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'被访对象姓名',
            align:"center",
            dataIndex: 'visitedname'
          },
          {
            title:'被访对象手机号码',
            align:"center",
            dataIndex: 'visitedpnum'
          },
          {
            title:'来访事由',
            align:"center",
            dataIndex: 'visitedevent'
          },
          {
            title:'来访时间',
            align:"center",
            dataIndex: 'visittime',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'离开时间',
            align:"center",
            dataIndex: 'lefttime',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'来访者姓名',
            align:"center",
            dataIndex: 'visitorname'
          },
          {
            title:'来访者手机号码',
            align:"center",
            dataIndex: 'visitorpnum'
          },
          {
            title:'身份证号',
            align:"center",
            dataIndex: 'identitynum'
          },
          {
            title:'车牌号',
            align:"center",
            dataIndex: 'platenum'
          },
          {
            title:'图片路径',
            align:"center",
            dataIndex: 'imgurl'
          },
          {
            title:'状态',
            align:"center",
            dataIndex: 'status'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/visit/visitinfo/list",
          delete: "/visit/visitinfo/delete",
          deleteBatch: "/visit/visitinfo/deleteBatch",
          exportXlsUrl: "/visit/visitinfo/exportXls",
          importExcelUrl: "visit/visitinfo/importExcel",

        },
        dictOptions:{},
        superFieldList:[],
      }
    },
    created() {
    this.getSuperFieldList();
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
    },
    methods: {
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'visitedname',text:'被访对象姓名'})
        fieldList.push({type:'string',value:'visitedpnum',text:'被访对象手机号码'})
        fieldList.push({type:'string',value:'visitedevent',text:'来访事由'})
        fieldList.push({type:'date',value:'visittime',text:'来访时间'})
        fieldList.push({type:'date',value:'lefttime',text:'离开时间'})
        fieldList.push({type:'string',value:'visitorname',text:'来访者姓名'})
        fieldList.push({type:'string',value:'visitorpnum',text:'来访者手机号码'})
        fieldList.push({type:'string',value:'identitynum',text:'身份证号'})
        fieldList.push({type:'string',value:'platenum',text:'车牌号'})
        fieldList.push({type:'string',value:'imgurl',text:'图片路径'})
        fieldList.push({type:'string',value:'status',text:'状态'})
        this.superFieldList = fieldList
      },
      loadData(status) {
      debugger;
        getAction(this.url.list, {status: this.status, pageNo : this.ipagination.current,
          pageSize :this.ipagination.pageSize}).then((res) => {
          if (res.success) {
            this.dataSource = res.result.records;
            this.ipagination.total = res.result.total;
          } else {
            this.dataSource = null;
          }
        })
      },
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>