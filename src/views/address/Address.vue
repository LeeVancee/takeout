<template>
  <Header title="地址管理" />
  <van-address-list
    :list="list"
    default-tag-text="默认"
    @add="onAdd"
    @edit="onEdit"
  />
</template>

<script setup>
import { onMounted, reactive, toRefs } from 'vue'
import Header from '../../components/Header.vue'
import { useRouter } from 'vue-router'
import { useMainStore } from '../../store'

const mainStore = useMainStore()
const router = useRouter()
let data = reactive({
  list: []
})
const { list } = toRefs(data)
// 初始化数据
const init = () => {
  data.list = mainStore.userAddress.map((item) => {
    return {
      id: item.id,
      name: item.name,
      tel: item.tel,
      address: `${item.province}${item.city}${item.county}${item.addressDetail}`,
      isDefault: !!item.isDefault
    }
  })
}
onMounted(() => {
  init()
})

// 新增地址的按钮
const onAdd = () => {
  router.push({
    path: './addressedit',
    query: {
      type: 'add'
    }
  })
}

// 编辑地址的按钮
const onEdit = (item) => {
  router.push({
    path: './addressedit',
    query: {
      id: item.id,
      type: 'change'
    }
  })
}
</script>

<style lang="less" scoped>
:deep(.van-button--danger) {
  background-color: #ffc400;
  border-color: #ffc400;
}
:deep(.van-switch--on) {
  background-color: #ffc400;
}
:deep(.van-radio__icon) {
  display: none;
}
</style>
