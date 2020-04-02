<template>
  <div class="user-manage">
    <div class="page-header">
      用户管理
    </div>
    <div class="page-body">
      <div class="btn-wrap">
        <a-button type="error" :disabled="delDisabled" @click="delAll">
          <span>删除</span>
          <a-badge :count="selectedRowKeys.length" class="badge-count" />
          <span v-show="selectedRowKeys.length">项</span>
        </a-button>
        <a-button type="dashed" @click="addNew">
          <font-awesome-icon
            :icon="['fas', 'plus']"
            style="margin-right: 4px;"
          />新的账户
        </a-button>
      </div>

      <a-table 
      :columns="columns" 
      :dataSource="userList" 
      :loading="isLoading" 
      :pagination="false"
      row-key="_id"
      :scroll="{ x: 985 }"
      :row-selection="rowSelection">
        <template slot="admin" slot-scope="text">
          {{text ? '是' : '否'}}
        </template>
        <template slot="createTime" slot-scope="text">
          {{text | toDate}}
        </template>
        <template slot="modifyTime" slot-scope="text">
          {{text | toDate}}
        </template>
        <template slot="action" slot-scope="text, row">
          <div class="action-td">
            <a-button
              title="编辑"
              :disabled="!row._id"
              @click="editUser(row)"
            >
              <font-awesome-icon
                :icon="['fas', 'pencil-alt']"
              />
            </a-button>
            <a-button
              title="删除"
              @click="del(row._id)"
            >
              <font-awesome-icon
                :icon="['far', 'trash-alt']"
              />
            </a-button>
          </div>
        </template>
      </a-table>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import moment from 'moment';
import { IResp } from '@/types';
import { IAuth } from '@/types/schema';


const columns = [
  {
    title: '账号',
    dataIndex: 'username',
    key: 'username'
  },
  {
    title: '密码',
    dataIndex: 'password'
  },
  {
    title: '管理员',
    dataIndex: 'isAdmin',
    key: 'isAdmin',
    width: 80,
    align: 'center',
    scopedSlots: { customRender: 'admin' },
  },
  {
    title: '创建时间',
    key: 'createTime',
    dataIndex: 'createTime',
    width: 160,
    align: 'center',
    scopedSlots: { customRender: 'createTime' }
  },
  {
    title: '修改时间',
    key: 'modifyTime',
    dataIndex: 'modifyTime',
    width: 160,
    align: 'center',
    scopedSlots: { customRender: 'modifyTime' }
  },
  {
    title: 'Action',
    key: 'action',
    fixed: 'right',
    align: 'center',
    width: 130,
    scopedSlots: { customRender: 'action' },
  },
];


export default Vue.extend({
  name: 'PageUserManage',
  layout: 'admin',
  data(){
    return {
      columns,
      userList:[],
      selectedRowKeys: [],
      isLoading: false
    }
  },
  computed: {
    delDisabled (): boolean {
      return this.selectedRowKeys.length === 0;
    },
    rowSelection (): object {
      return {
        selectedRowKeys: this.selectedRowKeys,
        onChange: selectedRowKeys => {
          this.selectedRowKeys = selectedRowKeys;
        },
        getCheckboxProps: record => ({
          props: {
            disabled: false//[allCategoryItem._id, this.otherCategoryId].includes(record._id)
          }
        })
      };
    }
  },
  created(){
    this.getUserList()
  },
  methods: {
    async getUserList(){
      this.isLoading=true
      const { code, data }: IResp = await this.$axios.$get('/api/admin/userList');
      if(code === 1){
        this.userList = data
      }else{
        this.$message.error('请求失败！');
      }
      this.isLoading = false
    },
    delAll ():void{

    },
    addNew():void {

    },
    editUser(row){

    },
    del(uid){

    }
  }
});
</script>

<style scoped>
  .action-td .ant-btn {
    width: 32px;
    padding: 0;
  }

  .ant-table-body {
    overflow-x: auto !important;
  }

  .btn-wrap{
    margin-bottom: 10px;
  }
</style>
