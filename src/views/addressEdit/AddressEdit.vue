<template>
  <Header :title="address" />
  <van-address-edit
    :area-list="areaList"
    :address-info="addressInfo"
    show-delete
    show-set-default
    :area-columns-placeholder="['请选择', '请选择', '请选择']"
    @save="onSave"
    @delete="onDelete"
  />
</template>

<script setup>
import { reactive, toRefs, onMounted, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import Header from '../../components/Header.vue'
import { useMainStore } from '../../store'
import { Toast } from 'vant'

const route = useRoute()
const router = useRouter()
const mainStore = useMainStore()
let data = reactive({
  areaList: {
    province_list: {
      110000: '广东省',
      120000: '浙江省'
    },
    city_list: {
      110100: '广州市',
      110200: '深圳市',
      120100: '杭州市',
      120200: '宁波市'
    },
    county_list: {
      110101: '天河区',
      110102: '海珠区',
      120102: '上城区',
      130102: '下城区'
    }
  },
  addressInfo: {}
})
const { areaList, addressInfo } = toRefs(data)
const address = computed(() => {
  return route.query.type === 'add' ? '地址新增' : '地址编辑'
})
// 数据初始化
const init = () => {
  mainStore.userAddress.forEach((item) => {
    if (item.id === Number(route.query.id)) {
      data.addressInfo = item
    }
  })
}

onMounted(() => {
  init()
})

//保存的按钮
const onSave = (content) => {
  if (route.query.type === 'add') {
    mainStore.ADDADDRESS(content)
  } else {
    mainStore.CHANGEADDRESS(content)
  }
  Toast('保存成功')
  setTimeout(() => {
    router.back()
  }, 1000)
}

//删除的按钮
const onDelete = (content) => {
  mainStore.DELETEADDRESS(content)
  Toast('删除成功')
  setTimeout(() => {
    router.back()
  }, 1000)
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
</style>
