<template>
  <!-- 基本信息 -->
  <div class="message">
    <!-- Logo -->
    <h1 class="messageh1">
      我是 <span id="typed">{{ obj.output }}</span>
    </h1>
    <h1>的前端开发工程师</h1>
    <!-- 简介 -->
    <div class="description cards" @click="changeBox">
      <div class="content">
        <Icon size="16">
          <QuoteLeft />
        </Icon>
        <div class="text">
          <p>{{ descriptionText.hello }}</p>
          <p>{{ descriptionText.text }}</p>
        </div>
        <Icon size="16">
          <QuoteRight />
        </Icon>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, watch, h, onMounted, ref } from 'vue'
import { Icon } from '@vicons/utils'
import { QuoteLeft, QuoteRight } from '@vicons/fa'
import { Error } from '@icon-park/vue-next'
import { mainStore } from '@/store'
import EasyTyper from '@/utils/typed.js'
const store = mainStore()

// // 主页站点logo
// let siteLogo = import.meta.env.VITE_SITE_LOGO
// // 站点链接
// let siteUrl = import.meta.env.VITE_SITE_URL.split('.')

// 简介区域文字
let descriptionText = reactive({
  hello: import.meta.env.VITE_DESC_HELLO,
  text: import.meta.env.VITE_DESC_TEXT,
})
//打印机效果
const obj = reactive({
  output: '',
  isEnd: false,
  speed: 300,
  singleBack: true,
  sleep: 10,
  type: 'rollback',
  backSpeed: 40,
  sentencePause: false,
})
const myString = ref(['有技术', '有颜值', '有深度', '有两把刷子'])
onMounted(() => {
  const typed = new EasyTyper(obj, myString.value, (instance) => {
    instance.input = ['有技术', '有颜值', '有深度', '有两把刷子']
    instance.play()
  })
})
// 切换右侧功能区
const changeBox = () => {
  if (store.getInnerWidth >= 990) {
    store.boxOpenState = !store.boxOpenState
  } else {
    ElMessage({
      message: '当前页面宽度不足以开启盒子',
      grouping: true,
      icon: h(Error, {
        theme: 'filled',
        fill: '#efefef',
      }),
    })
  }
}

// 监听状态变化
watch(
  () => store.boxOpenState,
  (value) => {
    if (value) {
      descriptionText.hello = import.meta.env.VITE_DESC_HELLO_OTHER
      descriptionText.text = import.meta.env.VITE_DESC_TEXT_OTHER
    } else {
      descriptionText.hello = import.meta.env.VITE_DESC_HELLO
      descriptionText.text = import.meta.env.VITE_DESC_TEXT
    }
  }
)
</script>

<style lang="scss" scoped>
.message {
  .messageh1 {
    height: 2.5rem;
  }
  .logo {
    display: flex;
    flex-direction: row;
    align-items: center;
    animation: fade;
    -webkit-animation: fade 0.5s;
    .logo-img {
      border-radius: 50%;
      width: 120px;
    }
    .name {
      width: 100%;
      height: 142px;
      margin-left: 12px;
      transform: translateY(-8px);
      font-family: 'Pacifico-Regular';

      .bg {
        font-size: 5rem;
      }

      .sm {
        margin-left: 6px;
        font-size: 2rem;
        @media (min-width: 720px) and (max-width: 789px) {
          display: none;
        }
      }
    }
    @media (max-width: 768px) {
      .logo-img {
        width: 100px;
      }
      .name {
        height: 128px;
        .bg {
          font-size: 4.5rem;
        }
      }
    }
  }

  .description {
    padding: 1rem;
    margin-top: 3.5rem;
    max-width: 460px;
    animation: fade;
    -webkit-animation: fade 0.5s;
    cursor: pointer;

    .content {
      display: flex;
      justify-content: space-between;

      .text {
        margin: 0.75rem 1rem;
        line-height: 2rem;
        margin-right: auto;

        p {
          &:nth-of-type(1) {
            font-family: 'Pacifico-Regular';
          }
        }
      }

      .xicon:nth-of-type(2) {
        align-self: flex-end;
      }
    }
    @media (max-width: 720px) {
      max-width: 100%;
      pointer-events: none;
    }
  }
  @media (max-width: 390px) {
    .logo {
      flex-direction: column;
      .logo-img {
        display: none;
      }
      .name {
        margin-left: 0;
        height: auto;
        transform: none;
        text-align: center;
        .bg {
          font-size: 3.5rem;
        }
        .sm {
          font-size: 1.4rem;
        }
      }
    }
    .description {
      margin-top: 2.5rem;
    }
  }
}
</style>
