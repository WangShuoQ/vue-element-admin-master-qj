<template>
        <div class="category">
            <h2>栏目管理</h2>
		<!-- 按钮 -->
		<div>
			<el-button @click="toAddHandler" size="small" type="primary" >添加</el-button>
			<el-button @click="batchDeleteHandler" size="small" type="danger">批量删除</el-button>
		</div>
		<!-- 表格 -->
		<div>
			<el-table :data= "categories" size="mini" @selection-change="handleSelectionChange" >
                <el-table-column type="selection" width="55"></el-table-column>
                <el-table-column prop="id" label="编号"></el-table-column>
                <el-table-column prop="name" label="栏目名称"></el-table-column>

                <el-table-column prop="num" label="序号"></el-table-column>

                <el-table-column prop="parentId" label="父栏目"></el-table-column>
 
                <el-table-column label="操作" width="60" align="center">
                     <template #default="record">
                         <i class="el-icon-delete" @click.prevent="deleteHandler(record.row.id)"></i>
                         &nbsp;
                         <i class="el-icon-edit-outline" @click.prevent="editHandler(record.row)"></i>
                     </template>
                </el-table-column>
            </el-table>    
		</div>
        <!-- 模态框 -->
        <el-dialog :title="title" :visible.sync="visible" @close="closeModal">
            <el-form :model="category" :rulers="rules" ref="categoryForm">
                <el-form-item label="栏目名称" label-width="100px" prop="realname">
                    <el-input v-model="category.name" auto-complete="off">
                    </el-input>
                </el-form-item>
                <el-form-item label="序号" label-width="100px" prop="num">
                    <el-input v-model="category.num" auto-complete="off">
                    </el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button size="small" @click="closeModal">取消</el-button>
                <el-button size="small" type="primary" @click="submitHandler">确定</el-button>
            </div>
        </el-dialog>
        <!-- 模态框 -->
        </div>
</template> 
<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
export default{
      data(){
          return{
              category:{},
              ids:[],
              rules:{
                  name:[
                      {required:true,message:'请输入栏目名称',trigger:'blur'},
                      {min:2,max:10,message:'长度在2到10个字符之间',trigger:'blur'}
                  ],
                  num:[
                      {required:true,message:'请输入序号',trigger:'blur'}
                  ]
              }
          }
      },
      computed:{
        // 映射 store->vue
        ...mapState("category",["categories","visible","title"]),
        ...mapGetters("category",["orderCategory","categorySize"])
    },
    created(){
        this.findAllCategories();
    },
    methods:{
        // 映射 store -> vue
        ...mapMutations("category",["showModal","closeModal","setTitle"]),
        ...mapActions("category",["findAllCategories","saveOrUpdateCategory","deleteCategoryById","batchDeleteCategory"]),
        // 普通方法
        handleSelectionChange(val){
            this.ids = val.map(item=>item.id);
        },
        toAddHandler(){
            // 1. 重置表单
            this.category = {};
            this.setTitle("添加栏目信息");
            // 2. 显示模态框
            this.showModal();
        },
        submitHandler(){
            this.$refs.categoryForm.validate((valid)=>{
                //如果校验通过
                if(valid){
                   let promise = this.saveOrUpdateCategory(this.category)
            promise.then((response)=>{
                // promise为action函数的返回值，异步函数的返回值就是promise的then回调函数的参数
                this.$message({type:"success",message:response.statusText});
            })  
                }else{
                    return false;
                }
            })
           
        },
        dialogCloseHandler(){
            this.$refs.categoryForm.resetFields();
        },
        editHandler(row){
            this.category = row;
            this.setTitle("修改栏目信息");
            this.showModal();
        },
        deleteHandler(id){
            this.deleteCategoryById(id)
            .then((response)=>{
               this.$message({type:"success",message:response.statusText});
            })
        },
        batchDeleteHandler(){
            this.batchDeleteCategory(this.ids)
            .then((response)=>{
                this.$message({type:"success",message:response.statusText});
            })
	    }

	}
}
</script>
<style scoped>

</style>