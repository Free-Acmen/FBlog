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
      <a-modal
        v-model="isModalShow"
        :title="modalTitle"
        :confirm-loading="confirmLoading"
        :closable="false"
        :destroy-on-close="true"
        @ok="saveUser"
      >
      <a-form label-position="top" :form="form">
        <a-form-item label="用户名" :colon="false">
          <a-input
            v-decorator="['userName', userNameOpts]"
            placeholder="请输入用户名"
            allow-clear
          />
        </a-form-item>
        <a-form-item label="密码" :colon="false">
          <a-input-password 
            v-decorator="['password', passwordOpts]"
            placeholder="请输入密码"
            allow-clear
          />
        </a-form-item>
        <a-form-item label="确认密码" :colon="false">
          <a-input-password 
            v-decorator="['rePassword', rePasswordOpts]"
            placeholder="请再次输入密码"
            allow-clear
          />
        </a-form-item>
      </a-form>
      
      </a-modal>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import moment from 'moment';
import md5 from 'blueimp-md5';
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
      isModalShow: false,
      confirmLoading: false,
      modalTitle:'新增用户',
      selectedRowKeys: [],
      isLoading: false,
    }
  },
  computed: {
    form () {
      return this.$form.createForm(this);
    },
    userNameOpts (): object {
      return {
        rules: [
          {required: true,message: '用户名不能为空！'},
          {validator: this.checkUsername}
        ]
      };
    },
    passwordOpts(): object {
      return {
        rules: [
          {required: true, message: '密码不能为空！'},
          {validator: this.checkPassword}
        ]
      }
    },
    rePasswordOpts(): object {
      return {
        rules: [
          {required: true, message: '密码确认不能为空！'},
          {validator: this.checkrePassword}
        ]
      }
    },
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
            disabled: false //[allCategoryItem._id, this.otherCategoryId].includes(record._id)
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
    checkUsername(_rule, value, callback){
      callback();
    },
    checkPassword(_rule, value, callback){
      var regx = /[1-9a-zA-Z]{6,10}/
      if(value && !regx.test(value)){
        callback("密码须由字母数字组成, 6-10个字符范围！");
      }else{
        callback();
      }
    },
    checkrePassword(_rule, value, callback){
      const form = (this as any).form;
      if (value && value !== form.getFieldValue('password')) {
        callback('两次输入的密码不一致！');
      } else {
        callback();
      }
    },
    delAll ():void{

    },
    addNew():void {
      this.isModalShow = true
    },
    editUser(row){

    },
    del(uid){

    },
    saveUser(){
      this.form.validateFieldsAndScroll((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values);
        }
      });
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
