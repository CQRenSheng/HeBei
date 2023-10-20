<template>
    <div>
        <Modal
            v-model="add.isshow"
            footer-hide
            :title="add.type=='add'?'新增项目':add.type=='detail'?'项目详情':'修改项目'"
            >
            <div>
                <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="120">
                    <FormItem label="项目负责人" prop="name">
                        <Input v-model="formValidate.name" placeholder="请填写项目名称"></Input>
                    </FormItem>
                    <FormItem label="项目负责人" prop="name">
                        <Input v-model="formValidate.principal" placeholder="请填写项目负责人"></Input>
                    </FormItem>
                    <FormItem label="负责人手机" prop="phone">
                        <Input v-model="formValidate.phone" placeholder="请填写项目负责人手机号"></Input>
                    </FormItem>
                    <FormItem label="核销人员" prop="pdaPerson">
                        <Select v-model="formValidate.pdaPerson" multiple style="width:260px">
                            <Option v-for="item in list.pdaPersonList" :value="item.id" :key="item.id">{{ item.person }}</Option>
                        </Select>
                    </FormItem>
                    <FormItem label="项目介绍" prop="introduce">
                        <Input v-model="formValidate.introduce" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请填写项目介绍"></Input>
                    </FormItem>
                    <FormItem>
                        <Button type="primary" @click="handleSubmit('formValidate')">确认</Button>
                        <Button @click="handleReset('formValidate')" style="margin-left: 8px">取消</Button>
                    </FormItem>
                </Form>
            </div>
        </Modal>
    </div>
</template>
<script>
    import data from '@/data.js'
    export default {
        name: 'addProject',
        data () {
            return {
                add:{
                    isshow:false,//控制组件是否显示
                    type:'add',//组件类型 新增，修改，查看  add,update,details
                },
                 //业务列表数据存放地
                 list:{
                    pdaPersonList:data.data,
                 },
                 //表单元素存放地
                formValidate:{
                    name:'',//项目名称
                    image:'',//项目图片
                    introduce:'',//项目介绍
                    phone:'',//负责人电话
                    status:'',//项目状态
                    pdaPerson:[],//PDA核销人员
                    principal:''//项目负责人
                },
                //表单校验存放地
                ruleValidate: {
                    name: [{ required: true, message: '请填写项目名称', trigger: 'blur' }],
                    phone: [{ required: true, message: '请填写项目负责人手机号码', trigger: 'blur' }],
                    pdaPerson: [{ required: true, type: 'array', min: 1, message: '请至少选择一个核销人员', trigger: 'change' },],
                    principal: [{ required: true, message: '请填写项目负责人', trigger: 'blur' }],
                }
            }
        },
        computed: {
           
        },
        methods: {
            init(data){//重置组件
                this.add.isshow = data.isshow
                this.add.type = data.type
                this.setdatas(data)
            },
            setdatas(data){//制空组件数据方法
                this.formValidate={
                    name:data.name?data.name:'',//项目名称
                    image:data.image?data.image:'',//项目图片
                    introduce:data.introduce?data.introduce:'',//项目介绍
                    phone:data.phone?data.phone:'',//负责人电话
                    status:data.status?data.status:'',//项目状态
                    pdaPerson:data.pdaPerson?data.pdaPerson:[],//PDA核销人员
                    principal:data.principal?data.principal:''//项目负责人
                }
            },
            handleSubmit (name){//提交表单
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        this.$Message.success('Success!');
                        this.add.isshow=false
                        this.$refs[name].resetFields();
                    }
                })
                
            },
            handleReset (name){//取消表单
                this.$refs[name].resetFields();
                this.add.isshow=false
            }
        },
        mounted () {
            
        },
        beforeDestroy () {
           
        }
    }
</script>

