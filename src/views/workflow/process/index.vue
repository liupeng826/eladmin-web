<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="主键ID" prop="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="流程名称">
            <el-input v-model="form.name" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="流程显示名称">
            <el-input v-model="form.displayName" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="流程类型">
            <el-input v-model="form.type" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="实例url">
            <el-input v-model="form.instanceUrl" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="流程是否可用">
            <el-input v-model="form.state" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="流程模型定义">
            <el-input v-model="form.content" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="版本">
            <el-input v-model="form.version" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="创建时间">
            <el-input v-model="form.createTime" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="创建人">
            <el-input v-model="form.creator" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="id" label="主键ID" />
        <el-table-column prop="name" label="流程名称" />
        <el-table-column prop="displayName" label="流程显示名称" />
        <el-table-column prop="type" label="流程类型" />
        <el-table-column prop="instanceUrl" label="实例url" />
        <el-table-column prop="state" label="流程是否可用" />
        <el-table-column prop="content" label="流程模型定义" />
        <el-table-column prop="version" label="版本" />
        <el-table-column prop="createTime" label="创建时间" />
        <el-table-column prop="creator" label="创建人" />
        <el-table-column v-permission="['admin','wfProcess:edit','wfProcess:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudWfProcess from '@/api/wfProcess'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, name: null, displayName: null, type: null, instanceUrl: null, state: null, content: null, version: null, createTime: null, creator: null }
export default {
  name: 'WfProcess',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: 'WorkFlowController', url: 'api/wfProcess', sort: 'id,desc', crudMethod: { ...crudWfProcess }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'wfProcess:add'],
        edit: ['admin', 'wfProcess:edit'],
        del: ['admin', 'wfProcess:del']
      },
      rules: {
        id: [
          { required: true, message: '主键ID不能为空', trigger: 'blur' }
        ]
      }}
  },
  methods: {
    // 获取数据前设置好接口地址
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
