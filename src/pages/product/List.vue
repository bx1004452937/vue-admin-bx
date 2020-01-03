<template>
    <div>
        <!-- 按钮 -->  
        <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>
        <!-- 按钮 --> 
        <!-- 表格 -->
        <el-table :data="product"> 
            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
    <el-table-column fixed="left" prop="name" label="产品名称"></el-table-column>
    <el-table-column fixed="left" prop="price" label="价格"></el-table-column>
    <el-table-column width="120" prop="description" label="描述"></el-table-column>
    <el-table-column width="120" prop="categoryId" label="所属产品"></el-table-column>
    <el-table-column fixed="right" label="操作">
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
             <el-form :model="form" label-width="80px">
      <el-form-item label="名称">
          <el-input v-model="form.name"/>
      </el-form-item>
            <el-form-item label="价格">
          <el-input type="price" v-model="form.price"/>
      </el-form-item>
      <el-form-item label="描述">
          <el-input v-model="form.description"/>
    
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
            product:[],
            form:{
                type:"product"
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
      let url = "http://localhost:6677/product/saveOrUpdate";
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
      let url = "http://localhost:6677/product/findAll"
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