<template>
  <div class="queue-list">
    <CommonForm v-bind="formOptions"></CommonForm>
    <CommonTable v-bind="tableOptions" v-on="$listeners">
      <!-- <slot v-for="slot in Object.keys($slots)" :name="slot" :slot="slot" /> -->
      <!-- {{ $slots }} -->
    </CommonTable>
  </div>
</template>

<script>
  import CommonTable from '@/components/CommonTable.vue'
  import CommonForm from '@/components/CommonForm.vue'
  export default {
    name: 'QueryList',
    components: {
      CommonTable,
      CommonForm
    },
    directives: {},
    filters: {},
    mixins: {},
    props: {
      formOptions: {
        required: true,
        type: Object
      },
      tableOptions: {
        required: true,
        type: Object
      }
    },
    data() {
      return {}
    },
    computed: {},
    watch: {},
    created() {},
    mounted() {},
    methods: {}
  }
</script>
<style lang="less"></style>
