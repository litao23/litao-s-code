<template>
    <div>
        <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>


        <el-table :data="employees">
            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column width="100" prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone" label="手机号"></el-table-column>
            <el-table-column prop="idCard" label="身份证号"></el-table-column>
            <el-table-column prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column prop="type" label="类型"></el-table-column>
            <el-table-column fixed="right" prop="id" label="操作">
                <template v-slot="slot">
                        <a href="" @click.prevent="toDeleteHandeler(slot.row.id)">删除</a>
                        <a href="" @click.prevent="toUpdateHandeler">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!--分页开始-->
        <el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>
        <!--分页结束-->
        <!--模态框开始-->
        <el-dialog
  :title.sync="title"
  :visible.sync="visible"
  width="60%">
  {{form}}
  <el-form :model="form" label-width="80px">
      <el-form-item label="用户名">
          <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
          <el-input  type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item label="姓名">
          <el-input   v-model="form.realname"></el-input>
      </el-form-item>
      <el-form-item label="性别">
        <el-radio-group v-model="form.gender">
            <el-radio label="男">男</el-radio>
            <el-radio label="女">女</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="电话号码">
          <el-input   v-model="form.telephone"></el-input>
      </el-form-item>
      <el-form-item label="身份证号">
          <el-input   v-model="form.idCard"></el-input>
      </el-form-item>
      <el-form-item label="银行卡号">
          <el-input   v-model="form.bankCard"></el-input>
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
import request from '@/utils/request' //第三方库
import querystring from 'querystring' //系统库
export default {
    //用于存放网页中需要的方法
    methods:{
        submitHandler(){
             let url="http://localhost:6677/waiter/saveOrUpdate"
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
            let url = "http://localhost:6677/waiter/findAll"
            //在methods方法中有this值
            request.get(url).then((response)=>{
        //将查询结果设置到customers中
        this.employees= response.data})
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
            this.title="修改员工信息";
            this.visible=true;
        },
        closeModalHandeler(){
            this.visible=false;
        },
        toAddHandler(){
            this.title="添加员工信息";
            this.visible=true;
        }
    },
    //用于存放要想网页中存放的数据
    data(){
        return{
            title:'录入员工信息',
            visible:false,
            employees:[],
            form:{
                type:"waiter"
            }
        }
    },
    created(){
        //在页面加载出来的时候加载数据
        this.loadData();
    }
}
</script>

<style scoped>

</style>