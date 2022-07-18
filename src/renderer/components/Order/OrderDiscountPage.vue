<template>
    <div class="content-page">
        <div class="content-nav">
            <el-breadcrumb class="breadcrumb" separator="/">
                <el-breadcrumb-item>优惠设置</el-breadcrumb-item>
            </el-breadcrumb>
            <div class="operation-nav">
                <el-button type="primary" icon="plus" @click="addDiscountTemplate">添加优惠</el-button>
            </div>
        </div>
        <div class="content-main">
            <div class="form-table-box">
                <el-table :data="tableData" style="width: 100%" border stripe>
                    <el-table-column prop="discount_id" label="ID" width="100px"></el-table-column>
                    <el-table-column prop="name" label="达人姓名"></el-table-column>
                    <el-table-column prop="code" label="优惠码"></el-table-column>
                    <el-table-column prop="discount" label="折扣" width="200"></el-table-column>
                    <el-table-column label="操作" width="170">
                        <template scope="scope">
                            <el-button size="small"  @click="handleRowEdit(scope.$index, scope.row)">编辑</el-button>
                            <el-button size="small" type="danger" @click="handleRowDelete(scope.$index, scope.row)">删除
                            </el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                tableData: []
            }
        },
        methods: {
            addDiscountTemplate(){
                this.$router.push({name: 'orderdiscount_add'});
            },
            handleRowEdit(index, row) {
                this.$router.push({name: 'orderdiscount_add', query: {id: row.discount_id}})
            },
            handleRowDelete(index, row) {
                this.$confirm('确定要删除?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.axios.post('order/destoryDiscount', {id: row.discount_id}).then((response) => {
                        console.log(response.data)
                        if (response.data.errno === 0) {
                            this.$message({
                                type: 'success',
                                message: '删除成功!'
                            });
                            this.getList();
                        }
                    })


                });
            },
            getList() {
                this.axios.get('order/discount').then((response) => {
                    // console.log(response);
                    this.tableData = response.data.data
                })
            }
        },
        components: {},
        mounted() {
            this.getList();
        }
    }

</script>

<style scoped>

</style>
