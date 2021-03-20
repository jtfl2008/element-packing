<script>
  export default {
    name: 'CommonForm',
    props: {
      model: {
        type: Object,
        default: () => ({})
      },
      groups: {
        type: [Boolean, Array],
        default: false
      },
      // 表单配置
      items: {
        // required: true,
        type: Array,
        default: () => []
      },
      inline: {
        type: Boolean,
        default: true
      },
      viewable: {
        type: Boolean,
        default: false
      },
      buttons: {
        type: [Boolean, Array],
        default: true
      }
    },
    data() {
      return {
        formModel: {},
        defaultWidth: '200'
      }
    },
    computed: {
      formName() {
        return this.name || 'common-form'
      },
      // 是否是表单组
      isGroup() {
        return this.groups && this.groups.length > 0
      }
    },
    watch: {},
    created() {
      this.formModelProcess()
    },
    mounted() {},
    methods: {
      // 表单Form节点数据处理
      formModelProcess() {
        let formModel = {}
        if (!this.model) {
          return
        }
        this.getFormItems().forEach(item => {
          let nameVal = item.prop
          if (!nameVal) {
            console.error('[form warn]: 存在未配置name或者name为空的表单项')
            return
          }
          // render 类型不添加属性,使用外部属性
          if (item.render) {
            return
          }
          formModel[nameVal] = this.checkValue(this.model[nameVal])
            ? this.model[nameVal]
            : this.getItemDefaultValue(item)
        })
        this.formModel = {
          ...formModel
        }
      },
      // 值为空判断
      checkValue(val) {
        return val || val === 0
      },
      getFormItems() {
        if (this.isGroup) {
          let groupItems = []
          this.groups.forEach(group => {
            groupItems = groupItems.concat(group.items || [])
          })
          return groupItems
        }
        return this.items || []
      },
      // 默认值
      getItemDefaultValue(item) {
        return item.value
          ? item.value
          : item.component === 'checkbox' ||
            (item.component === 'date-picker' && item.type === 'daterange') ||
            (item.component === 'select' && item.type === 'multiple')
          ? []
          : ''
      },
      // 生成选项列表
      generateList(item) {
        return item.options.map(option => {
          let ele = ''
          switch (item.component) {
            case 'select':
              ele = (
                <el-option
                  key={option.value}
                  label={option[item.labelKey] || option.label}
                  value={option[item.valueKey] || option.value}
                ></el-option>
              )
              break
            case 'checkbox':
              ele =
                item.type === 'button' ? (
                  <el-checkbox-button label={option.value}>
                    {option.label}
                  </el-checkbox-button>
                ) : (
                  <el-checkbox label={option.value}>{option.label}</el-checkbox>
                )
              break
            case 'radio':
              ele =
                item.type === 'button' ? (
                  <el-radio-button label={option.value}>
                    {option.label}
                  </el-radio-button>
                ) : (
                  <el-radio label={option.value}>{option.label}</el-radio>
                )
              break
          }
          return ele
        })
      },
      // 生成下拉菜单
      generateSelect(item) {
        return (
          <el-select
            v-model={this.formModel[item.prop]}
            {...{
              props: {
                ...this.$attrs,
                ...item
              }
            }}
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          >
            {this.generateList(item)}
          </el-select>
        )
      },
      // 生成多选框
      generateCheckbox(item) {
        return (
          <el-checkbox-group
            v-model={this.formModel[item.prop]}
            {...{
              props: {
                ...this.$attrs,
                ...item
              }
            }}
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          >
            {this.generateList(item)}
          </el-checkbox-group>
        )
      },
      // 生成单选
      generateRadio(item) {
        return (
          <el-radio-group
            v-model={this.formModel[item.prop]}
            {...{
              props: {
                ...this.$attrs,
                ...item
              }
            }}
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          >
            {this.generateList(item)}
          </el-radio-group>
        )
      },
      // 生成输入框
      generateInput(item) {
        return (
          <el-input
            v-model={this.formModel[item.prop]}
            clearable
            type={item.component}
            // disabled={item.disabled}
            // autosize={item.autosize}
            {...{
              props: {
                ...this.$attrs,
                ...item
              }
            }}
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          >
            {item.slots &&
              item.slots.map(list => {
                return (
                  <el-button
                    slot={list.name}
                    on-click={() => {
                      list.click()
                    }}
                  >
                    {list.label}
                  </el-button>
                )
              })}
          </el-input>
        )
      },
      generateDatePicker(item) {
        return (
          <el-date-picker
            v-model={this.formModel[item.prop]}
            clearable
            {...{
              props: {
                ...this.$attrs,
                'range-separator': '至',
                'start-placeholder': '开始日期',
                'end-placeholder': '结束日期',
                format: 'yyyy-MM-dd',
                'value-format': 'yyyy-MM-dd ',
                ...item
              }
            }}
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          ></el-date-picker>
        )
      },
      // 生成文本
      generateText(item) {
        return (
          <span
            class="el-form-item-text"
            style={{ width: (item.width || this.defaultWidth) + 'px' }}
          >
            {this.formModel[item.prop]}
          </span>
        )
      },
      // 分组
      generateGroup() {
        return this.groups.map(group => {
          return (
            <el-card>
              <div slot="header">
                <span>{group.title}</span>
              </div>
              {this.generateFormItems(group.items)}
            </el-card>
          )
        })
      },
      // 生成表单项
      generateFormItems(items = []) {
        let itemEle = ''
        return items.map(item => {
          // 浏览
          if (this.viewable) {
            let value = ''
            if (item.component === 'select' || item.component === 'radio') {
              value = this.getLabel(
                this.formModel[item.prop],
                item.options,
                item.labelKey || undefined,
                item.valueKey || undefined
              )
            } else if (item.component === 'checkbox') {
              let values = []
              item.options.forEach(option => {
                let models = this.formModel[item.prop]
                models.forEach(list => {
                  if (list === option.value) {
                    values.push(option)
                  }
                })
              })
              value = values.map(list => <span> {list.label} </span>)
            } else if (
              item.component === 'date-picker' &&
              item.type === 'daterange'
            ) {
              value = this.formModel[item.prop].join(' - ')
            } else {
              value = this.formModel[item.prop]
            }
            itemEle = (
              <span
                class="el-form-item-text"
                style={{ width: (item.width || this.defaultWidth) + 'px' }}
              >
                {value}
              </span>
            )
          } else {
            if (item.render) {
              itemEle = item.render()
            } else {
              switch (item.component) {
                // 下拉菜单
                case 'select':
                  itemEle = this.generateSelect(item)
                  break
                // 多选框
                case 'checkbox':
                  itemEle = this.generateCheckbox(item)
                  break
                // 单选框
                case 'radio':
                  itemEle = this.generateRadio(item)
                  break
                // 输入框
                case 'input':
                  itemEle = this.generateInput(item)
                  break
                // 日期
                case 'date-picker':
                  itemEle = this.generateDatePicker(item)
                  break
                // 文本
                case 'text':
                  itemEle = this.generateText(item)
                  break
                default:
                  itemEle = (
                    <item.component
                      v-model={this.formModel[item.prop]}
                      {...{
                        props: {
                          ...this.$attrs,
                          ...item
                        }
                      }}
                    />
                  )
              }
            }
          }
          if (item.hidden) return
          return (
            <el-form-item label={item.label} prop={item.prop}>
              {itemEle}
            </el-form-item>
          )
        })
      },
      // 按钮列表
      generateBtnList() {
        if (!this.buttons || this.buttons.length === 0) {
          console.error('[oa form warn]: 未配置表单按钮')
          return
        }
        return this.buttons.map(btn => {
          return (
            <el-button
              {...{
                props: {
                  ...this.$attrs,
                  type: btn.type || 'default'
                },
                on: {
                  click: () => {
                    btn.click(this.$refs[this.formName], this.formModel)
                  }
                }
              }}
            >
              {btn.text}
            </el-button>
          )
        })
      },
      getLabel(value, items, labelKey, valueKey) {
        let item =
          items.find(item => (item[valueKey] || item.value) === value) || ''
        if (!item) return
        return item[labelKey] || item.label
      }
    },
    render() {
      let ele = []
      // 表单内容
      if (this.isGroup) {
        ele = this.generateGroup()
      } else {
        ele = this.generateFormItems(this.items)
      }
      return (
        <el-form
          ref={this.formName}
          // model={this.formModel}
          // rules={this.rules}
          // inline={this.inline}
          // disabled={this.isDisabled}
          // label-width={this.labelWidth || '150px'}

          {...{
            props: {
              model: this.formModel, //vue jsx element 表单校验的model不可以直接写 以这种方式解决
              rules: this.rules,
              inline: this.inline,
              labelPosition: this.labelPosition,
              disabled: this.disabled,
              labelWidth: this.labelWidth || 'auto'
            }
          }}
        >
          {ele}
          <el-form-item
            label-width={this.isGroup ? '0' : this.labelWidth}
            class="el-button-group"
          >
            {this.generateBtnList()}
            <label class="title-tips">{this.btnTips || ''}</label>
          </el-form-item>
        </el-form>
      )
    }
  }
</script>

<style lang="less" scoped>
  .el-form {
    .el-card {
      margin-bottom: 20px;
    }
    .el-form-item-text {
      display: inline-block;
    }
  }
</style>
