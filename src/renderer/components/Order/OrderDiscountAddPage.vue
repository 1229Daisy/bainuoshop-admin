<template>
  <div class="content-page">
      <div class="content-nav">
            <el-breadcrumb class="breadcrumb" separator="/">
                <el-breadcrumb-item>添加/修改优惠</el-breadcrumb-item>
            </el-breadcrumb>
            
        </div>
    <div class="content-main">
      <div class="form-table-box">
                <el-form  label-width="120px" ref="infoForm"  :model="infoForm" :data="tableData">
                    <el-form-item prop="discount_id" label="ID" width="100px" v-if="infoForm.discount_id!=0">
                        <el-input  placeholder="id" v-model="infoForm.discount_id"></el-input>
                    </el-form-item>
                    <el-form-item label="达人姓名" prop="name">
                        <el-input  placeholder="请输入达人姓名" v-model="infoForm.name"></el-input>
                    </el-form-item>
                    <el-form-item label="优惠码" prop="code">
                        <el-input placeholder="请输入优惠码" v-model="infoForm.code"></el-input>
                    </el-form-item>

                    <el-form-item label="折扣" prop="discount">
                        <el-input  placeholder="请输入小数如打折20%应输入0.2" v-model="infoForm.discount"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSaveDiscount">确定保存</el-button>
                    </el-form-item>
                </el-form>

            </div>
    </div>
  </div>
</template>

<script>
export default {
    data() {
            return {
               infoForm: {
                    discount_id: 0,
                    code: '',
                    discount: '',
                    name: ''
                },
                tableData: [],
            }
        },
        methods: {
            onSaveDiscount() {
                let name = this.infoForm.name;
                let code = this.infoForm.code;
                let discount = this.infoForm.discount;

                // console.log(this.tableData);

                if (name == '') {
                    this.$message({
                        type: 'error',
                        message: '达人姓名必填'
                    })
                    return false;
                }

                if (code == '') {
                    this.$message({
                        type: 'error',
                        message: '优惠码必填'
                    })
                    return false;
                }
                if (discount == '') {
                    this.$message({
                        type: 'error',
                        message: '折扣必填'
                    })
                    return false;
                }
                if (discount > 1) {
                    this.$message({
                        type: 'error',
                        message: '折扣请填小数'
                    })
                    return false;
                }

                // let formData = JSON.stringify(this.infoForm);
                this.axios.post('order/saveDiscount', {
                    info: this.infoForm
                }).then((response) => {
                    console.info(this.infoForm)
                    if (response.data.errno === 0) {
                        this.$message({
                            type: 'success',
                            message: '保存成功'
                        });
                        this.getInfo();
                    } else {
                        this.$message({
                            type: 'error',
                            message: '保存失败'
                        })
                    }
                })
            },
             getInfo() {
                if (this.infoForm.discount_id <= 0) {
                    return false
                }
                //加载快递公司详情
                let that = this
                this.axios.post('order/discountShow', {
                    id: that.infoForm.discount_id
                }).then((response) => {
                    that.infoForm = response.data.data
                    that.tableData = response.data.data
                })
            }
            
        },
        components: {},
        mounted() {
            this.infoForm.discount_id = this.$route.query.id || 0;
            //  console.log(this.infoForm.id);
            this.getInfo();
        }
}
</script>

<style>

</style>