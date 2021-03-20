<script>
  export default {
    name: 'commonTable',
    props: {
      toolbar: {
        type: [Boolean, Array],
        default: false
      },
      columns: {
        required: true,
        type: Array,
        default: () => []
      },
      data: {
        required: true,
        type: Array,
        default: () => []
      },
      loading: {
        type: Boolean,
        default: false
      },
      index: {
        type: Boolean,
        default: true
      },
      selection: {
        type: Boolean,
        default: false
      },
      pagination: {
        type: [Object, Boolean],
        default: true
      },
      total: {
        type: Number,
        default: 0
      }
    },
    render() {
      return (
        <div class="table-pagination">
          {this.toolbar && (
            <div class="table-toolbar">
              {this.toolbar.map(button => {
                return (
                  <el-button
                    {...{
                      props: {
                        ...this.$attrs,
                        type: button.type || 'primary'
                      },
                      on: {
                        click: () => {
                          button.click(button)
                        }
                      }
                    }}
                  >
                    {button.text}
                  </el-button>
                )
              })}
            </div>
          )}
          <el-table
            border
            v-loading={this.loading}
            element-loading-text="加载中"
            data={this.data}
            {...{
              on: {
                ...this.$listeners
              }
            }}
          >
            {this.index && (
              <el-table-column
                type="index"
                label="序号"
                align="center"
                fixed="left"
                width={50}
              ></el-table-column>
            )}
            {this.selection && (
              <el-table-column
                type="selection"
                align="center"
                fixed="left"
                width={50}
              ></el-table-column>
            )}
            {this.columns.map(column => {
              let props = {
                ...this.$attrs,
                ...column,
                'min-width': column.width || '80',
                'show-overflow-tooltip': true
              }
              let columnProps = {
                props
              }
              if (props.render) {
                columnProps.scopedSlots = {
                  // ...columnProps,
                  default: scope => props.render(scope)
                  // header: scope => props.render(h, scope)
                }
              } else if (props.slot) {
                columnProps.scopedSlots = {
                  // ...columnProps,
                  default: scope => {
                    const params = {
                      column: scope.column,
                      $index: scope.$index,
                      row: scope.row
                    }
                    console.log(this.$scopedSlots)
                    console.log(this.$slots)
                    // debugger
                    if (!Object.keys(this.$slots)) {
                      return this.$scopedSlots[props.slot](params)
                    }
                  }
                }
              } else if (props.prop === 'operation') {
                columnProps = {
                  ...columnProps,
                  scopedSlots: {
                    default: scope => {
                      return props.buttons.map(button => {
                        return (
                          <el-button
                            on-click={() => {
                              button.click(scope)
                            }}
                          >
                            {button.label}
                          </el-button>
                        )
                      })
                    }
                  },
                  props: {
                    ...columnProps.props,
                    fixed: 'right',
                    'show-overflow-tooltip': false
                  }
                }
              }
              return (
                <el-table-column
                  key={column.prop}
                  {...columnProps}
                ></el-table-column>
              )
            })}
          </el-table>
          {this.pagination && (
            <el-pagination
              layout="total, sizes, prev, pager, next, jumper"
              page-sizes={[10, 20, 30, 40, 50, 100, 200]}
              page-size={this.pager.pageSize}
              current-page={this.pager.currentPage}
              total={this.total}
              on-current-change={val => {
                // this.$emit('update:currentPage', val)
                // this.$emit('page-current-change', val)
                this.$emit('page-change', {
                  ...this.pager,
                  currentPage: val
                })
              }}
              on-size-change={val => {
                // this.$emit('update:pageSize', val)
                // this.$emit('page-size-change', val)
                this.$emit('page-change', {
                  ...this.pager,
                  currentPage: 1,
                  pageSize: val
                })
              }}
              // {...{
              //   props: {
              //     layout: 'total, sizes, prev, pager, next, jumper',
              //     'page-sizes': [10, 20, 30, 40, 50, 100],
              //     'page-size': 10,
              //     'current-page': 1,
              //     total: this.total
              //   },
              //   on: {
              //     'current-change': val => {
              //       this.$emit('page-current-change', val)
              //     },
              //     'size-change': val => {
              //       this.$emit('page-size-change', val)
              //     }
              //   }
              // }}
            ></el-pagination>
          )}
        </div>
      )
    }
  }
</script>
<style lang="less">
  .table-pagination {
    .table-toolbar {
      margin-bottom: 20px;
    }
    .el-table {
      width: 100%;
    }
    .el-pagination {
      text-align: right;
      margin: 20px 0;
    }
  }
</style>
