<template>
  <div class="user-manage">
    <div class="page-header">
      用户管理
    </div>
    <div class="page-body">
      15121
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { IResp } from '@/types';
import { defaultSetting } from '@/server/models/setting';
export default Vue.extend({
  name: 'PageUserManage',
  layout: 'admin',
  data(){
    return {

    }
  }
});
</script>

<style scoped>

</style>
