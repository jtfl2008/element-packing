<template>
  <div class="home">
    <QueryList
      :form-options="formOptions"
      :table-options="tableOptions"
      @page-change="onPageChange"
    >
      <!-- <template #sex="{row}">
        <div>插槽示例 {{ row.sex }}</div>
      </template>
      <template #sex1="{row}">
        <div>插槽示例 {{ row.sex }}</div>
      </template> -->
    </QueryList>
    <!-- <CommonForm :form-options="formOptions"></CommonForm> -->
    <!-- <CommonTable :table-options="tableOptions"> -->
    <!-- <CommonTable v-bind="tableOptions">
      <template #sex="{row}">
        <div>插槽示例 {{ row.sex }}</div>
      </template>
    </CommonTable> -->
  </div>
</template>

<script>
  // @ is an alias to /src
  import QueryList from '@/components/QueryList.vue'
  // import CommonTable from '@/components/CommonTable.vue'
  // import CommonForm from '@/components/CommonForm.vue'
  export default {
    name: 'Home',
    components: {
      QueryList
      // CommonTable
      // CommonForm
    },
    data() {
      return {
        tableData: [
          {
            date: '2020-03-27',
            name: '老村长',
            address: '四川省成都市青羊区人民中路一段16号'
          }
        ],
        config: [{ label: '时间', slot: 'filterTime' }],
        tableOptions: {
          toolbar: [
            {
              text: '新增',
              click: this.onCreade
            }
          ],
          data: [
            {
              date: '2020-03-27',
              sex: '♂',
              name: '老村长',
              address: '四川省成都市青羊区人民中路一段16号'
            },
            {
              date: '2020-03-27',
              sex: '♀',
              name: '老村长',
              address: '四川省成都市青羊区人民中路一段16号'
            },
            {
              date: '2020-03-27',
              sex: '男',
              name: '老村长',
              address: '四川省成都市青羊区人民中路一段16号'
            },
            {
              date: '2020-03-27',
              sex: '女',
              name: '老村长1',
              address: '四川省成都市青羊区人民中路一段16号'
            }
          ],
          columns: [
            // { prop: 'index', label: '序号', align: 'center', type: 'index' },
            // { prop: 'selection', align: 'center', type: 'selection' },

            { prop: 'date', label: '日期' },
            {
              slot: 'sex',
              label: '性别',
              render: ({ row }) => <span> {row.sex} </span>
            },
            {
              prop: 'name',
              label: '姓名',
              render: ({ row }) => {
                return <i> 自定义渲染 {row.name} </i>
              }
            },
            {
              prop: 'address',
              label: '地址',
              width: '120',
              formatter: row => {
                return `格式化${row.address}...`
              }
            },
            {
              prop: 'operation',
              label: '操作',
              // width: '160',
              buttons: [
                {
                  label: '修改',
                  click: this.handlerEditor
                },
                {
                  label: '查看1',
                  click: this.handlerEditor
                }
              ]
            }
          ],
          total: 1230
        },

        formOptions: {
          model: {
            // biz_type11: 2,
            biz_type11: '#409EFF',
            biz_type1: 1
            // biz_type2: ''
          },
          items: [
            {
              label: '类型11',
              prop: 'biz_type11',
              component: 'el-color-picker'
              // labelKey: 'text',
              // valueKey: 'index',
            },
            {
              label: '类型1',
              prop: 'biz_type1',
              component: 'select',
              labelKey: 'text',
              valueKey: 'index',
              options: [
                {
                  index: 1,
                  text: '业务部'
                }
              ]
            }
            // {
            //   label: '类型2',
            //   prop: 'biz_type2',
            //   render: () => {
            //     return (
            //       <el-input
            //         v-model={this.formOptions.model.biz_type2}
            //       ></el-input>
            //     )
            //   }
            // }
          ],
          // rules: {
          //   biz_type: [
          //     { required: true, message: '请输入产品名称', trigger: 'blur' }
          //   ]
          // },
          buttons: [
            {
              text: '搜索',
              type: 'primary',
              click: this.commitForm
            },
            {
              text: '重置',
              type: 'default',
              click: this.resetForm
            }
          ]
        },
        formOptions2: {
          data: {
            address: '12',
            sex: '男',
            sex2: '男',
            name: '老六',
            age: '',
            biz_type1: 1,
            biz_type2: [1, 2],
            biz_type3: 1,
            date: '2021-02-16'
          },
          groups: [
            {
              title: '基本信息',
              items: [
                {
                  label: '展示',
                  prop: 'sex',
                  component: 'text'
                },
                {
                  label: '输入',
                  prop: 'name',
                  component: 'input',
                  // hidden: true,
                  slots: [
                    {
                      name: 'append',
                      label: '按钮',
                      click: this.clickAppend
                    }
                  ]
                },

                {
                  label: '选择',
                  prop: 'biz_type1',
                  component: 'select',
                  options: [
                    {
                      value: 1,
                      label: '业务部1'
                    },
                    {
                      value: 2,
                      label: '技术部1'
                    }
                  ]
                },
                {
                  label: '复选框',
                  prop: 'biz_type2',
                  component: 'checkbox',
                  disabled: true,
                  options: [
                    {
                      value: 1,
                      label: '业务部2'
                    },
                    {
                      value: 2,
                      label: '技术部2'
                    }
                  ]
                },
                {
                  label: '单选框',
                  prop: 'biz_type3',
                  component: 'radio',
                  type: 'button',
                  disabled: true,
                  options: [
                    {
                      value: 1,
                      label: '业务部3',
                      id: 1,
                      name: '业务部3'
                    },
                    {
                      value: 2,
                      label: '技术部3',
                      id: 1,
                      name: '业务部3'
                    }
                  ]
                },
                {
                  label: '日期',
                  prop: 'date',
                  // type: 'daterange',
                  component: 'date-picker'
                },
                {
                  label: '自定义渲染',
                  prop: 'address',
                  render: param => {
                    console.log(param)
                    return (
                      <el-input
                        {...{
                          props: {
                            value: param
                          },
                          on: {
                            input: val => {
                              console.log(
                                '🚀 ~ file: Home.vue ~ line 230 ~ data ~ val',
                                val
                              )

                              this.value = val
                            }
                          }
                        }}
                      ></el-input>
                    )
                  }
                }
              ]
            },
            {
              title: '基本信息2',
              items: [
                {
                  label: '展示',
                  prop: 'sex2',
                  component: 'text'
                }
              ]
            }
          ],

          rules: {
            biz_type: [
              { required: true, message: '请输入产品名称', trigger: 'blur' }
            ]
          },
          buttons: [
            {
              text: '保存',
              type: 'primary',
              click: this.commitForm
            },
            {
              text: '重置',
              type: 'danger',
              click: this.resetForm
            }
          ]
        }
      }
    },
    methods: {
      getDataList() {
        let params = {
          ...this.pager,
          ...this.formOptions.data
        }
        console.log(params)
      },
      handlerEditor(val) {
        console.log('handlerEditor -> val', val)
      },
      onPageChange(pager) {
        this.pager = pager
        this.getDataList()
      },

      onSelectionChange(val) {
        console.log(val)
      },
      commitForm(form, val) {
        console.log(form, val)
        form.validate(valid => {
          if (valid) {
            // console.log({ ...val, biz_type2: this.formOptions.data.biz_type2 })
          }
        })
      },
      resetForm(form) {
        this.formOptions.data.biz_type2 = ''
        form.resetFields()
        this.getDataList()
      },
      clickAppend() {},
      onCreade() {
        console.log('created')
      }
    }
  }
</script>
