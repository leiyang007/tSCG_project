<template>
	<div class="edit-role">
		<div class="nav-list">
			<span class="red-font">Production Planner</span>/
			<a href="javascript:history.go(-1)">
				<span class="yellow-font">Commodity</span>
			</a>
			/<span class="gray-font bh-title">Add Commodity</span>
		</div>
		<div class="edit-cont">
			<el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="demo-ruleForm">
				<el-row style="margin-bottom: 0;">
				  <el-col :span="4">
				  	<div class="cont-left">
				  		<span>Department:</span>					
				  	</div>
				  </el-col>
				  <el-col :span="20">
				  	<div class="cont-right">
				  		<el-form-item label="" prop="departmentId">
				  			<el-select v-model="ruleForm.departmentId" placeholder="Please choose" @change="changeDepartment" style="width: 200px;">
							    <el-option
							      v-for="item in departmentList"
							      :key="item.id"
							      :label="item.name"
							      :value="item.id">
							    </el-option>
							</el-select>
				  		</el-form-item>
				  	</div>
				  </el-col>
				</el-row>
				<el-row style="margin-bottom: 0;">
				  <el-col :span="4">
				  	<div class="cont-left">
				  		<span>Commodity:</span>					
				  	</div>
				  </el-col>
				  <el-col :span="20">
				  	<div class="cont-right">
				  		<el-form-item label="" prop="commodity">
				  			<el-input v-model="ruleForm.commodity" placeholder="Please input contents" style="width: 300px;"></el-input>
				  		</el-form-item>
				  	</div>
				  </el-col>
				</el-row>
				<el-row>
				  <el-col :span="4">
				  	<div class="cont-left">
				  		<span>Description:</span>					
				  	</div>
				  </el-col>
				  <el-col :span="20">
				  	<div class="cont-right">
				  		<el-input type="textarea" v-model="description" placeholder="Please input description" style="width: 300px;"></el-input>
				  	</div>
				  </el-col>
				</el-row>
				<el-row>
				  <el-col :span="4">
				  	<div class="cont-left"></div>
				  </el-col>
				  <el-col :span="20">
				  	<div class="cont-right">
				  		<div style="margin: 30px 0 50px;">
							<el-button type="danger" @click="submitForm('ruleForm')">Save</el-button>
							<a href="javascript:history.go(-1)">
								<el-button type="info" style="margin-left: 10px;">Cancel</el-button>
							</a>
						</div>
				  	</div>
				  </el-col>
				</el-row>
			</el-form>
		</div>
		
	</div>
</template>

<script>
export default{
	data(){
		return{
			ruleForm: {
				departmentId: null,
				commodity: null,
			},			
			description: null,
			departmentList: [],
			rules: {
	          commodity: [
	            { required: true, message: 'Please input a valid commodity Name!', trigger: ['blur', 'change'] }
	          ],
	          departmentId: [
	            { required: true, message: 'Please choose one department!', trigger: 'change' }
	          ],
	        }
		}
	},	
	methods:{
		getDepartmentList(){
			this.$axios.get('/qitems/queryDepartmentList')
				
			  .then((res) => {			  	
			  	let nowData = res.data
			  	if(nowData.code == 1){
			  		this.departmentList = nowData.data
			  	}else{
			  		console.log(nowData.errorInfo)
			  		this.$message({
			          message: nowData.errorInfo,
			          type: 'warning'
			        })
			  	}
			  	
			  })
			  .catch((err) => {
			    console.log(err);
			    this.$message({
		          message: err,
		          type: 'warning'
		        })
			  })
		},
		submitForm(formName) {
	        this.$refs[formName].validate((valid) => {
	          if (valid) {
	            this.saveRole()
	          } else {
	            console.log('error submit!!');
	            return false;
	          }
	        });
	    },
	    changeDepartment(selVal){
			console.log(selVal)
		},
        saveRole(){
        	let _this = this
        	let parm = {       		  
				  "departmentId": this.ruleForm.departmentId,
				  "name": this.ruleForm.commodity,
				  "description": this.description
			}
			console.log(parm)		
			this.$axios.post('/v1/category', parm)
			  .then((res) => {			  	
			  	let nowData = res.data

			  	if(nowData.code == 1){	
			  		console.log(JSON.stringify(nowData.data))
			  		this.$message({
			          message: 'Successfully Saved！',
			          type: 'success'
			        })
			  		setTimeout(function(){
			       		_this.$router.push('/tscg_system/commodity')
			        },1000)
			  	}else{
			  		console.log(nowData.errorInfo)
			  		this.$message({
			          message: nowData.errorInfo,
			          type: 'warning'
			       })
			  	}
			  	
			  })
			  .catch((err) => {
			    console.log(err);
			    this.$message({
		          message: err,
		          type: 'warning'
		        })
			  })
       },
	},
	mounted (){
		this.getDepartmentList()
	}	
}	
</script>

<style lang="less" scoped>	
.edit-role .edit-cont{
	padding-top: 20px;
	.cont-left{
	text-align: right; padding-right: 20px; margin-top: 10px;
	}
	.cont-right{
		text-align: left; padding-left: 20px;
	}
	.el-row{
		margin-bottom: 20px;
	}		
	.role-table {
	    border: 1px solid #e0e0e0;
	    border-bottom: none;
	    padding: 0;
	    position: relative;
	}	
	.header {
	    height: 40px;
	    line-height: 40px;
	    border-bottom: 1px solid #e7e7e7;
	    background: #F8F8F9;
	    text-align: center;
	}	
	.vertical-line {
	    width: 1px;
	    height: 100%;
	    background: #ddd;
	    position: absolute;
	    left: 30%;
	    top: 0
	}	
	.left {
	    width: 28%;
	    float: left;
	    padding-left: 10px;
	    user-select: none;
	    cursor: pointer;
	}	
	.one {
	    padding-left: 20px;
	}	
	.right {
	    width: 70%;
	    float: left;
	    padding-left: 30px;
	    box-sizing: border-box;
	}	
	.item-icon {
	    margin-left: -5px;
	    padding: 5px;
	}	
	.line {
	    clear: both;
	    width: 100%;
	    height: 1px;
	    background: #e0e0e0;
	}
	.h40{
	    height: 39px;
	    line-height: 39px;
	}
	[v-cloak] {
	    display: none;
	}
	.role-li{
		padding-bottom: 3px;
	}
	.el-checkbox+.el-checkbox{
	    margin-left: 0px;
	}
	.el-checkbox{
		margin-right: 30px;
	}
	

}
</style>