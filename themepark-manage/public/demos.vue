<template>
    <div>
        <m-page-header title="项目管理">
            <Form ref="TextForm" slot="content" :model="serch" :label-width="100" :show-message="false">
                <Row :gutter="24" type="flex" >
                    <Col v-bind="grid">
                        <FormItem label="协议名：" prop="title" label-for="title">
                            <Input v-model="serch.protocolTitle" element-id="title" placeholder="请输入协议名" maxlength="20" show-word-limit/>
                        </FormItem>
                    </Col>
                    <Col v-bind="grid" offset="8" class="ivu-text-right" style="align-items: flex-end;">
                        <FormItem class="ivu-mb-0">
                            <Button type="primary" icon="ios-filing" @click="save">保存</Button>
                            <Button type="error" class="ivu-ml-8" icon="md-close" @click="clear">清空文档</Button>
                        </FormItem>
                    </Col>
                </Row>
            </Form>
            <RadioGroup v-model="size" type="button" slot="action" size="small" @on-change="handleChangeSize">
                <Radio v-font="16" label="large">大</Radio>
                <Radio v-font="14" label="default">中</Radio>
                <Radio v-font="12" label="small">小</Radio>
            </RadioGroup>
        </m-page-header>
        <Card :bordered="false" class="ivu-mt" dis-hover>
            <Table ref="table" class="ivu-mt" :data="table.data" :columns="table.columns" :loading="table.loading"
                   :size="size" border>
                <template slot-scope="{row, index}" slot="scenes">
                    <span v-if="row.scenes === 'HEAT'">热力</span>
                    <span v-if="row.scenes === 'SCHOOL'">学校</span>
                    <span v-if="row.scenes === 'TRAIN'">培训机构</span>
                </template>
                <template slot-scope="{row, index}" slot="status">

                    <Switch :value="row.status" :loading="row.statusLoading" true-value="1" false-value="0"
                            :before-change="handleBeforeChange"
                            @mousedown.native="switchChange( row, index)"
                            size="large">
                        <span slot="open">启用</span>
                        <span slot="close">停用</span>
                    </Switch>
                </template>
                <template slot-scope="{row, index}" slot="aggregate">
                    <Tag v-if="row.mchntNo == null" color="orange">未开通</Tag>
                    <Tag v-else color="green">已开通</Tag>
                </template>
                <template slot-scope="{row, index}" slot="action">
                    <Divider type="vertical"/>
                    <Tooltip content="编辑" placement="top" transfer>
                        <a>
                            <Icon type="ios-create" color="#515a6e" size="18" @click="handleRow('edit', row, index)"/>
                        </a>
                    </Tooltip>
                    <Divider type="vertical"/>

                    <Tooltip content="删除" placement="top" transfer>
                        <Poptip confirm transfer :title="`是否要删除商户: ${row.name} 吗？`" @on-ok="handleRow('del', row, index)">
                            <a><Icon type="ios-trash" size="18" color="#515a6e"/></a>
                        </Poptip>
                    </Tooltip>
                </template>
            </Table>
            <m-page :total="page.total" :current.sync="page.current" :pageSize.sync="page.pageSize" :size="size"
                    @changePage="getData" @handleChangePageSize="getData"></m-page>
        </Card>
    </div>
</template>
<script>
    import {  } from '@api/TicketInformation/project';//引入定义的api
    import _ from 'lodash';//引入js类库
    import mixinPage from '@/mixins/page';//混入列表底部页签
    import mixinSearchForm from '@/mixins/searchForm';//混入搜索栏
    import { mapState } from 'vuex';//引入vux状态管理
    import data from '@/data.js'//引入模拟数据
    export default {
        name: 'TicketInformation-project',
        mixins: [mixinPage, mixinSearchForm],
        data () {
            return {
                serch: {
                    pkId: '',
                    protocolTitle: '',
                    useProtocol: false,
                    protocol: ''
                },
                table: {
                    data: [],
                    loading: false,
                    columns: [
                        {
                            type: 'index',
                            width: 55,
                            align: 'center',
                            indexMethod: (row) => {
                                return (row._index + 1) + (this.page.pageSize * this.page.current) - this.page.pageSize;
                            }
                        },
                        {
                            title: '商户名称',
                            key: 'name',
                            align: 'center',
                            minWidth: 230
                        },
                        {
                            title: '操作',
                            slot: 'action',
                            align: 'center',
                            width: 190,
                            fixed: 'right'
                        }
                    ]
                },
            }
        },
        mounted () {
            this.getData();
        },
        methods: {
            async getData () {
                this.table.data=data.data
            },
            async save () {
                this.$Message.success('保存');
            },
            async clear () {
                this.$Message.warning('清空')
            }
        },
        computed: {
            ...mapState('admin/user', [
                'organizations',
                'info'
            ]),
        },
        components: {
            'mTinymce': () => import('@/components/m-tinymce')
        }
    }
</script>
