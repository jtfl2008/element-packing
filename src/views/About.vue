<template>
  <div>
    <!-- <my-button type="primary" @click="onClick('primary')">primary</my-button>
    <my-button type="danger" @click="onClick('danger')">danger</my-button>
    <MyButton :loading="true">button</MyButton> -->

    <!-- [1,2,[3,[4,5]]]嵌套数组去重   [1,2,3,4,5] -->
  </div>
</template>

<script>
  // import MyButton from '@/components/MyButton.vue'
  export default {
    name: 'About',
    components: {
      // MyButton
    },
    props: {},
    data() {
      return {
        arr: [1, 2, [3, [4, 5]]]
      }
    },
    computed: {},
    watch: {},
    mounted() {
      // this.flatten
      this.myFilter()
    },
    methods: {
      onClick(type) {
        console.log(type)
      },
      flatten() {
        let arr = [1, 2, [3, [4, 5]]]
        let newArr = []
        for (let i = 0; i < arr.length; i++) {
          if (Array.isArray(arr[i])) {
            this.flatten(arr[i])
          } else {
            newArr.push(arr[i])
          }
        }
        return newArr
      },
      myFilter() {
        let arr = [
          { id: 1, name: '张三' },
          { id: 3, name: '李四' },
          { id: 2, name: '李四' }
        ]

        let arr1 = arr.sort((a, b) => {
          return a.id - b.id
        })

        let arr2 = []

        arr1.forEach(item => {
          if (!arr2.includes(list => item.name === list.name)) {
            debugger
            arr2.push(item)
          }
        })
        console.log(arr2)
        return arr2
      }
    }
  }
</script>

<style></style>
