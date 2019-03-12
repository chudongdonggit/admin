<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 类别管理</el-breadcrumb-item>
                <el-breadcrumb-item>查看类别</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            {{data5}}
        </div>
        <div class="container">
            <el-tree
                    :data="data5"
                    draggable
                    node-key="id"
                    default-expand-all
                    :expand-on-click-node="false">
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button
                  type="text"
                  size="mini"
                  @click="() => append(data)">
            添加
          </el-button>
            <el-button v-show="data.id!=0"
                    type="text"
                    size="mini"
                    @click="() => append(data)">
            修改
          </el-button>
          <el-button  v-show="data.id!=0"
                  type="text"
                  size="mini"
                  @click="() => remove(node, data)">
            删除
          </el-button>
        </span>
      </span>
            </el-tree>

            <el-dialog title="新增类别" :visible.sync="dialogVisible">
                <el-form :model="form">
                    <el-form-item label="父级名称" :label-width="'120px'">
                        {{parent.label}}
                    </el-form-item>
                    <el-form-item label="类别名称" :label-width="'120px'">
                        <el-input v-model="form.name" autocomplete="off"></el-input>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="closeDialog">取 消</el-button>
                    <el-button type="primary" @click="addClass">确 定</el-button>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
  export default {
    name: "ViewClass",
    data:function () {
      return {
        data5:[{
          id: 0,
          label: '新闻分类(根)',
          children: []
        }],
        dialogVisible:false,
        parent:{},
        form:{
          name:'',
          parentId:0,
        }
      }
    },
    methods:{
      append(data) {
        this.parent = data;
        this.form.parentId = data.id;
        this.dialogVisible = true;
        // const newChild = { id: id++, label: 'testtest', children: [] };
        // if (!data.children) {
        //   this.$set(data, 'children', []);
        // }
        // data.children.push(newChild);
      },
      closeDialog:function () {
        this.dialogVisible = false;
      },
      addClass:function () {
        const _this = this;
        this.$axios.post('http://127.0.0.1:3000/interface/addclass',this.form)
          .then(function (result) {
          if(result.data.flag){
            console.log(result.data.data);
            _this.data5 = result.data.data;
          }else{
            alert(result.data.msg)
          }
            _this.form.name = '';
            _this.dialogVisible = false;
        })
      }
    }
  }
</script>

<style scoped>
    .custom-tree-node {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 14px;
        padding-right: 8px;
    }
</style>
