<template>
    <div>
        <!-- 按钮 -->  
        <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>
        <!-- 按钮 --> 
        <!-- 表格 -->
        <el-table :data="employees"> 
            <el-table-column  fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column  fixed="left" prop="username" label="用户名"></el-table-column>
            <el-table-column  fixed="left" prop="realname" label="姓名"></el-table-column>
            <el-table-column  prop="gender" label="性别"></el-table-column>
            <el-table-column  width="120" prop="telephone" label="联系方式"></el-table-column>
            <el-table-column  width="200" prop="idCard" label="身份证号"></el-table-column>
            <el-table-column  width="200" prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column  fixed="right" label="操作">
                <template v-slot="slot">
                 <a href="" @click.prevent="toDeletHander(slot.row.id)">删除 </a>    
                 <a href="" @click.prevent="toUpdataHander(slot.row)">修改 </a>         
                 </template>
            </el-table-column>
        </el-table>
        <!-- 表格 -->
        <!-- 分页 -->
        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <!-- 分页 -->
        <!-- 模态框 -->
        <el-dialog
            :title="title"
            :visible.sync="visible"
            width="60%">
            {{form}}
            <el-form :model = "form" label-width="80px">
                <el-form-item label="用户名">
                    <el-input v-model="form.username"/>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"/>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="form.realname"/>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="form.gender">
                        <el-radio label="男">男</el-radio>
                        <el-radio label="女">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="手机号">
                    <el-input v-model="form.telephone"/>
                </el-form-item>
                <el-form-item label="身份证号">
                    <el-input v-model="form.idCard"/>
                </el-form-item>
                <el-form-item label="银行卡号">
                    <el-input v-model="form.bankCard"/>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModelHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
      </span>
        </el-dialog>
        <!-- 模态框 -->

    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
                type:"waiter"
            }
        }
    },
    created(){
        this.loadData();
    },
    methods:{
        submitHandler(){
      //this.form 对象 ---字符串--> 后台 {type:'customer',age:12}
      // json字符串 '{"type":"customer","age":12}'
      // request.post(url,this.form)
      // 查询字符串 type=customer&age=12
      // 通过request与后台进行交互，并且要携带参数
      let url = "http://localhost:6677/waiter/saveOrUpdate";
      request({
        url,
        method:"POST",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data:querystring.stringify(this.form)
      }).then((response)=>{
        // 模态框关闭
        this.closeModelHandler();
        // 刷新
        this.loadData();
        // 提示消息
        this.$message({
          type:"success",
          message:response.message
        })
      })

    },
        loadData(){
      let url = "http://localhost:6677/waiter/findAll"
      request.get(url).then((response)=>{
        // 将查询结果设置到customers中，this指向外部函数的this
        this.employees = response.data;
      })
    },
        toAddHandler(){
            this.title="录入员工信息";
            this.visible=true;
        },
        toUpdataHander(row){
            this.title="修改员工信息";
            this.visible=true;
        },
        toDeletHander(id){
            {
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                this.$message({
                    type: 'success',
                    message: '删除成功!'
                });          
            });
        }
        },
        closeModelHandler(){
            this.visible=false;
        },
        
    }
}
</script>

<style scoped>

</style>





