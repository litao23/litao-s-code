<template>
    <div>
        <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">删除</el-button>
        <el-table :data="categorys">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="项目名称"></el-table-column>
            <el-table-column prop="num" label="价格"></el-table-column>
            <el-table-column width="200px" prop="icon" label="图片"></el-table-column>
            <el-table-column prop="parentId" label="所属产品"></el-table-column>
            <el-table-column fixed="right" label="操作">
                <template v-slot="slot">
                        <a href="" @click.prevent="toDeleteHandeler(slot.row.id)">删除</a>
                        <a href="" @click.prevent="toUpdateHandeler">修改</a>
                </template>
            </el-table-column> 
        </el-table>
        <!--表格结束-->
        <!--分页开始-->
        <el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>
        <!--分页结束-->
        <!--模态框开始-->
<el-dialog
  title="录入项目信息"
  :visible.sync="visible"
  width="60%">
  {{form}}
  <el-form :model="form" label-width="80px">
      <el-form-item label="项目">
          <el-input   v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="价格">
          <el-input   v-model="form.num"></el-input>
      </el-form-item>
      <el-form-item label="图片">
          <el-input   v-model="form.icon"></el-input>
      </el-form-item>
  </el-form>

  <span slot="footer" class="dialog-footer">
    <el-button @click="closeModalHandeler " size="small">取 消</el-button>
    <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
  </span>
</el-dialog>
        <!--模态框结束-->
    </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    //用于存放网页中需要的方法
    methods:{
        toAddHandler(){
            let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.options = response.data;
            })
            this.title="添加产品信息",
            this.visible=true;
        },
        toDeleteHandeler(id){
            //确认
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
        toUpdateHandeler(){
            this.visible=true;
        },
        closeModalHandeler(){
            this.visible=false;
        },
        submitHandler(){
            //对表单中的数据进行储存
            let url="http://localhost:6677/category/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                //请求结束
                this.closeModalHandeler();
                this.loadData();
                this.$message({
                    type:"success",
                    message:response.message
                })
                })
        },
        loadData(){
            let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
        //将查询结果设置到customers中
        this.categorys = response.data})
        }
    },
    data(){
        return{
            visible:false,
            categorys:[],
            form:{
                type:"category"
            }
        }
    },
    created(){
        //this为当前vue实例
        //vue实例创建完毕
    this.loadData();
    }
}
</script>
<style scoped>

</style>