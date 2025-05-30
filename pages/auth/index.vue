<script setup lang="ts">
import Login from '@/components/Authorization/Login.vue'
import Register from '@/components/Authorization/Register.vue'
import { ref, computed } from 'vue'

definePageMeta({
    layout: 'auth',
})

useHead({
    title: '登录',
})

const router = useRouter()

router.beforeEach((to, from, next) => {
    if (to.path === '/auth' || from.path === '/auth') {
        window.location.href = to.fullPath
    } else {
        next()
    }
})

const route = useRoute()
const RegToken = computed(() => (route.query.token as string) || undefined)

const activeKey = ref('login')

// 如果存在 token，则默认显示注册组件
onMounted(() => {
    if (RegToken.value) {
        activeKey.value = 'register'
    }
})
</script>

<template>
    <div class="layout">
        <div class="l_body">
            <h1>MSCPO</h1>
            <h2>管理实用程序</h2>
        </div>
        <div class="r_body">
            <a-tabs v-model:activeKey="activeKey">
                <a-tab-pane key="login" tab="登录">
                    <Login />
                </a-tab-pane>
                <a-tab-pane key="register" tab="注册">
                    <Register :token="RegToken" />
                </a-tab-pane>
            </a-tabs>
        </div>
    </div>
</template>

<style scoped lang="less">
.layout {
    box-sizing: border-box;
    height: 100%;
    /* 建议在 nuxt.config.ts 中预加载背景图片 */
    background-image: url('@/assets/bg.webp');
    background-size: cover;
    background-position: center;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: row;

    /* 优化重绘提示 */
    will-change: transform, opacity;

    .l_body {
        box-sizing: border-box;
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        will-change: transform, opacity;

        h1 {
            text-align: center;
            font-size: 50px;
            font-weight: bold;
            color: transparent;
            background: linear-gradient(to right, #32defd, #f74598);
            -webkit-background-clip: text;
            background-clip: text;
            opacity: 0;
            animation: fadeIn 1s forwards;
            will-change: opacity, transform;
        }

        h2 {
            text-align: center;
            font-size: 25px;
            color: white;
            opacity: 0;
            animation: fadeIn 1.5s forwards;
            will-change: opacity, transform;
        }
    }

    .r_body {
        box-sizing: border-box;
        width: 400px;
        padding: 2rem;
        background-color: rgba(255, 255, 255, 0.6);
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        margin-right: 8rem;
        opacity: 0;
        animation: fadeIn 2s forwards;
        will-change: opacity, transform;
    }

    @keyframes fadeIn {
        0% {
            opacity: 0;
            transform: translateY(20px);
        }
        100% {
            opacity: 1;
            transform: translateY(0);
        }
    }

    @media screen and (max-width: 768px) {
        flex-direction: column;
        padding: 20px;

        .l_body {
            padding-bottom: 20px;
        }

        .r_body {
            width: 100%;
            margin-right: 0;
        }
    }
}
</style>
