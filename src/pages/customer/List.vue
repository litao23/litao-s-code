<template>
    <div>
        <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">删除</el-button>

        <el-table :data="customers">
            <el-table-column  prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone" label="联系方式"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                        <a href="" @click.prevent="toDeleteHandeler(slot.row.id)">删除</a>
                        <a href="" @click.prevent="toUpdateHandeler(slot.row)">修改</a>
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
  title="录入顾客信息"
  :visible.sync="visible"
  width="60%">
  <el-form :model="form" label-width="80px">
      <el-form-item label="用户名">
          <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
          <el-input  type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item label="真实姓名">
          <el-input   v-model="form.realname"></el-input>
      </el-form-item>
      <el-form-item label="电话号码">
          <el-input   v-model="form.telephone"></el-input>
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
        loadData(){
            let url = "http://localhost:6677/customer/findAll"
            request.get(url).then((response)=>{
        //将查询结果设置到customers中
        this.customers = response.data})
        },
        submitHandler(){
            //对表单中的数据进行储存
            let url="http://localhost:6677/customer/saveOrUpdate"
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
        toUpdateHandeler(row){
            this.form=row;
            this.visible=true;

        },
        closeModalHandeler(){
            this.visible=false;
        },
        toAddHandler(){
            this.visible=true;
        }
    },
    //用于存放要想网页中存放的数据
    data(){
        return{
            visible:false,
            customers:[],
            form:{
                type:"customer"
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