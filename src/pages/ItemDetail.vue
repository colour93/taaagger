<template>
    <div class="container">
        <el-card :body-style="{ padding: '0px' }" v-loading="loading">
            <div class="item-img-box"><el-image class="item-img" :fit="'contain'" :src="img" /></div>

            <div class="item-detail">
                <span class="item-title">{{ name }}</span>
                <table>
                    <tr>
                        <td>物品编号</td>
                        <td>{{ id }}</td>
                    </tr>
                    <tr>
                        <td>物品状态</td>
                        <td>{{ status == 'normal' ? "正常" : status == 'lost' ? "丢失" : "未知" }}</td>
                    </tr>
                    <tr>
                        <td>物品归属</td>
                        <td><el-link :href="owner_homepage" type="primary">{{ owner }}</el-link></td>
                    </tr>
                    <tr>
                        <td>联系方式</td>
                        <td><el-link :href="'tel:' + contact" type="primary">{{ contact }}</el-link></td>
                    </tr>
                    <tr>
                        <td>录入日期</td>
                        <td>{{ (new Date(create_date)).toLocaleDateString() }}</td>
                    </tr>
                    <tr>
                        <td>更新日期</td>
                        <td>{{ (new Date(update_date)).toLocaleDateString() }}</td>
                    </tr>
                </table>
            </div>
        </el-card>
    </div>
</template>

<style scoped>
p {
    margin: 0;
}

.container {
    min-height: calc(100vh - 20px * 2);
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.item-img-box {
    display: flex;
    justify-content: center;
}

.item-img {
    width: 60vw;
    height: 60vw;
    max-width: 320px;
    max-height: 320px;
}

.item-detail {
    padding: 14px;
    display: flex;
    flex-direction: column;
}

.item-title {
    font-size: large;
    font-weight: 600;
}

.item-detail-list {
    font-size: 16px;
}

.item-detail-list td {
    padding: 3px 4px;
}
</style>

<script setup>

import { ref, onMounted } from 'vue'
import { ElMessage } from 'element-plus';

import 'element-plus/es/components/message/style/css'

const loading = ref(true)

const id = ref("###")
const name = ref("###")
const img = ref("###")
const status = ref("###")
const owner = ref("###")
const owner_homepage = ref("###")
const contact = ref(0)
const create_date = ref(1600000000000)
const update_date = ref(1600000000000)

onMounted(async () => {

    const baseUrl = "https://fur93.oss-cn-beijing.aliyuncs.com/taaagger"

    const targetId = (new URLSearchParams(window.location.search)).get("id")

    if (!targetId) {

        ElMessage.error("请输入正确的参数")
        return 

    }

    let resp = await fetch(baseUrl + '/data/' + targetId + ".json")
    let data = await resp.json()

    if (data.id) {

        id.value = data.id
        name.value = data.name
        img.value = baseUrl + '/img/' + data.img
        status.value = data.status
        owner.value = data.owner
        owner_homepage.value = data.owner_homepage
        contact.value = data.contact
        create_date.value = data.create_date
        update_date.value = data.update_date

        loading.value = false

        ElMessage.success("数据获取成功")
        
    } else {

        ElMessage.error("数据获取失败，请刷新重试")

    }
    
});

</script>