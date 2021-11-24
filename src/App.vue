<script>
import { ref, nextTick } from 'vue';
import blockView from './components/block.vue';

export default {
  components: {
    blockView,
  },
  setup() {
    const boxColor = ref('border-color: #ccc');
    const blockhash = ref('');
    const blockJson = ref(null);
    const searching = ref(false);

    // 查询数据
    async function search() {
      if (searching.value) return;
      searching.value = true;
      const res = await fetch('https://blockchain.info/rawblock/' + blockhash.value);
      const json = await res.json();
      console.log(json);
      searching.value = false;
      blockJson.value = json;
    }

    nextTick(() => {
      console.log(location.pathname);
      if (location.pathname.trim() !== '/') {
        blockhash.value = location.pathname.slice(1);
        search();
      }
    });

    return {
      boxColor,
      blockhash,
      blockJson,
      searching,
      search,
    };
  },
};
</script>

<template>
  <div class="view">
    <form class="search-view" v-on:submit.prevent>
      <label for="block_hash" class="search" :style="boxColor">
        <input
          id="block_hash"
          type="text"
          @focus="boxColor = `border-color: #0c6cf2`"
          @blur="boxColor = `border-color: #ccc`"
          maxlength="100"
          placeholder="请输入Block Hash"
          v-model="blockhash"
        />
      </label>
      <button :class="['btn', searching ? 'searching' : '']" @click="search"></button>
    </form>
    <transition name="fade">
      <block-view :blockJson="blockJson" v-if="blockJson" />
    </transition>
  </div>
</template>

<style>
@import './assets/common.css';
.view {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 1rem;
  box-sizing: border-box;
}
.search-view {
  width: 600px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 3rem;
}
.search-view .search,
.search-view .btn {
  height: 2.8rem;
  box-sizing: border-box;
}
.search-view .search {
  display: flex;
  align-items: center;
  flex: 1;
  border: 1px solid #ccc;
  border-right: none;
  border-radius: 0.6rem 0 0 0.6rem;
  overflow: hidden;
  padding: 0 1rem;
  transition: 0.5s linear all;
}
.search-view .search input {
  width: 100%;
  height: 2.4rem;
  border: 0;
  color: #333;
  font-size: 1rem;
}
.search-view .btn {
  position: relative;
  width: 3.4rem;
  flex-shrink: 1;
  border: none;
  border-radius: 0 0.6rem 0.6rem 0;
  background: rgb(12, 108, 242);
  cursor: pointer;
  transition: 0.2s linear all;
}
.search-view .btn::before {
  content: '搜';
  position: absolute;
  left: 50%;
  top: 50%;
  width: 1.4rem;
  height: 1.4rem;
  text-align: center;
  line-height: 1.4rem;
  transform: translate3d(-50%, -50%, 0);
  color: #fff;
  font-family: 'iconfont';
  font-size: 1.2rem;
}
.search-view .btn.searching {
  background: rgb(12, 108, 242, 0.4);
}
.search-view .btn.searching::before {
  animation: rotate linear 1.2s infinite;
}
.search-view .btn:hover {
  font-size: 1.3rem;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
@keyframes rotate {
  0%,
  100% {
    transform: translate3d(-40%, -50%, 0);
  }
  25% {
    transform: translate3d(-50%, -60%, 0);
  }
  50% {
    transform: translate3d(-60%, -40%, 0);
  }
  75% {
    transform: translate3d(-50%, -40%, 0);
  }
}
@media screen and (max-width: 500px) {
  .search-view {
    width: 100%;
  }
}
</style>
