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
      :dataSource="tableData" 
      :loading="isLoading" 
      :pagination="false"
      :scroll="{ x: 785 }"
      row-key="_id"
      :row-selection="rowSelection">
        <template slot="name" slot-scope="text">{{text}}</template>
        <template slot="customTitle"><a-icon type="smile-o" /> Name</template>
        <template slot="tags" slot-scope="tags">
          <a-tag
            v-for="tag in tags"
            :color="tag==='loser' ? 'volcano' : (tag.length > 5 ? 'geekblue' : 'green')"
            :key="tag"
          >
            {{tag.toUpperCase()}}
          </a-tag>
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
  import { IResp } from '@/types';
  import { IAuth } from '@/types/schema';


  const columns = [
    {
      dataIndex: 'name',
      key: 'name',
      slots: { title: 'customTitle' },
      scopedSlots: { customRender: 'name' },
    },
    {
      title: 'Age',
      dataIndex: 'age',
      key: 'age',
    },
    {
      title: 'Address',
      dataIndex: 'address',
      key: 'address',
    },
    {
      title: 'Tags',
      key: 'tags',
      dataIndex: 'tags',
      scopedSlots: { customRender: 'tags' },
    },
    {
      title: 'Action',
      key: 'action',
      fixed: 'right',
      scopedSlots: { customRender: 'action' },
    },
  ];

  const tableData = [
    {
      _id: 12121213332,
      key: '1',
      name: 'John Brown',
      age: 32,
      address: 'New York No. 1 Lake Park',
      tags: ['nice', 'developer'],
    },
    {
      _id: 12121212,
      key: '2',
      name: 'Jim Green',
      age: 42,
      address: 'London No. 1 Lake Park',
      tags: ['loser'],
    },
    {
      _id: 121212333,
      key: '3',
      name: 'Joe Black',
      age: 32,
      address: 'Sidney No. 1 Lake Park',
      tags: ['cool', 'teacher'],
    },
  ];

export default Vue.extend({
  name: 'PageUserManage',
  layout: 'admin',
  data(){
    return {
      columns,
      tableData,
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
  methods: {
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
</style>
