<script setup>
import { ref } from 'vue'
import { useLogStore } from '@/stores/log-store'
import { useBitmapStore } from '@/stores/bitmap-store'
import router from '@/router'
import assets from '@/game/assets'
import { useLocalStorage } from '@vueuse/core'

const logStore = useLogStore()
const bitmapStore = useBitmapStore()

const currentAssetsMod = useLocalStorage('assets-mod', { index: 0 })
const loading = ref(false)
const start = async () => {
  loading.value = true
  // logStore.logger.info(`效果包：${assets[currentAssetsMod.value.index].info}`)
  logStore.logger.info('加载图片...')
  await bitmapStore.loadBitmaps()
  logStore.logger.info('所有资源加载完毕！')
  logStore.loaded = true
  return router.push('/play')
}

</script>

<template>
  <div class="start-frame">
    <h1 class="start-title">飞机大战</h1>
    <p class="logger-info">{{ logStore.log }}</p>
    <div v-if="loading" class="start-btn">加载中...</div>
    <template v-else>
      <button class="start-btn" @click="start">进入游戏</button>
      <!-- <label>
        音效和图标包：
        <select v-model="currentAssetsMod.index" name="currentAssetsMod">
          <option v-for="({name,info},i) in assets" :key="name" :value="i">{{ info }}</option>
        </select>
      </label> -->
    </template>
    <ul class="introduction">
      <li>鼠标单击画面后操控飞机，再次单击解除控制</li>
      <li>Q 散射：发射几圈环形子弹清除周围的敌人（狂暴效果：增强发射速度）</li>
      <li>W 治疗：持续治疗一段时间（狂暴效果：提高治疗量）</li>
      <li>E 狂暴：提高子弹伤害和范围，开启狂暴后开启其他技能有所加强</li>
      <li>R 时停，暂停时间，期间免疫伤害（狂暴效果：清除所有敌机子弹）</li>
    </ul>
  </div>
</template>

<style scoped lang="postcss">
.start-frame {
  width: 100%;
  height: 100%;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  .start-title {
    font-size: 48px;
    color: #333;
  }

  .start-btn {
    margin: 30px;
    border: none;
    padding: 10px 15px;
    font-size: 28px;
    color: #333;
    cursor: pointer;
    background: white;
    border-radius: 6px;
  }

  .introduction {
    padding: 1em;
    margin: 0 auto;
    list-style: none;
  }

  .logger-info {
    color: #666;
    font-size: 14px;
  }

  .link {
    margin-top: 20px;
  }
}
</style>
