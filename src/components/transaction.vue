<!--
 * @Author: yaohui.hou
 * @Date: 2021-11-22 16:05:17
 * @LastEditTime: 2021-11-23 14:33:40
 * @Description: 交易记录
-->
<template>
  <div class="transaction">
    <ul class="list">
      <li v-for="item in list" :key="item.hash">
        <!-- fee -->
        <div class="row">
          <div class="fee left">
            <span class="title">Fee</span>
            <var>{{ BTCfun(item.fee) }}</var>
          </div>
          <div class="btc right">
            <span class="title">Amount</span>
            <div class="total">
              <em class="outnum">{{ total(item.out) }}</em>
            </div>
          </div>
        </div>
        <!-- hash -->
        <div class="row">
          <div class="hash left">
            <span class="title">Hash</span>
            <a :href="`https://www.blockchain.com/btc/tx/${item.hash}`">{{ item.hash }}</a>
          </div>
          <div class="timer right">
            <span class="title">Date</span>
            <time>{{ transformTimer(item.time) }}</time>
          </div>
        </div>
        <!-- 交易清单 -->
        <div class="row">
          <div class="inputs-view left">
            <span class="title">From</span>
            <ol class="inputs">
              <li v-for="(input, i) in item.inputs" :key="'input' + i">
                <p v-if="input.prev_out">
                  <a :href="`https://www.blockchain.com/btc/tx/${input.prev_out.addr}`">{{ input.prev_out.addr }}</a>
                  <var>{{ BTCfun(input.prev_out.value) }}</var>
                </p>
                <p v-else class="nothing">COINBASE (Newly Generated Coins)</p>
              </li>
            </ol>
          </div>
          <div class="outs-view right">
            <span class="title">To</span>
            <ol class="outs">
              <li v-for="(outitem, e) in item.out" :key="'outitem' + e">
                <p>
                  <a :href="`https://www.blockchain.com/btc/address/${outitem.addr}`" v-if="outitem.addr">{{
                    outitem.addr
                  }}</a>
                  <span v-else>OP_RETURN</span>
                  <var>{{ BTCfun(outitem.value) }}</var>
                </p>
              </li>
            </ol>
          </div>
        </div>
      </li>
    </ul>
    <!-- 分页器 -->
    <div class="pager">
      <span @click="setPage(-1)">左</span>
      <form v-on:submit.prevent>
        <input type="number" v-model="page" :max="transactionlist.lenght" min="1" />
        <button @click="getList">GO</button>
      </form>
      <span @click="setPage(1)">右</span>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  props: ['transactionlist'],
  setup(props) {
    const list = ref(null);
    const page = ref(1);
    const amount = 5;
    function getList() {
      const start = (page.value - 1) * amount;
      const end = page.value * amount;
      list.value = props.transactionlist.slice(start, end);
      console.log(list.value);
      window.scrollTo({
        top: 0,
        behavior: 'smooth',
      });
    }
    getList();
    function BTCfun(n) {
      return (n / 100000000).toFixed(8) + 'BTC';
    }
    function transformTimer(t) {
      return new Date(parseInt(t) * 1000).toLocaleString().replace(/\//g, '-');
    }
    function total(arr) {
      let sum = 0;
      arr.forEach((item) => {
        sum += item.value;
      });
      return BTCfun(sum);
    }
    function setPage(n) {
      console.log(n);
      if (page.value + n <= 0) return;
      page.value += n;
      getList();
    }
    return {
      page,
      list,
      BTCfun,
      transformTimer,
      total,
      getList,
      setPage,
    };
  },
};
</script>
<style scoped>
.transaction {
  max-width: 980px;
  margin: 0 auto;
  font-size: 0.96rem;
}
a {
  overflow: hidden;
  text-overflow: ellipsis;
}
.transaction .list > li {
  display: flex;
  flex-direction: column;
  margin-bottom: 3rem;
}
.transaction .list > li .row {
  width: 100%;
  display: flex;
  flex: 1;
  justify-content: space-between;
}
.list > li .row .left,
.list > li .row .right {
  width: 50%;
  display: flex;
}
.transaction .list > li .title {
  width: 100px;
  flex-shrink: 0;
  padding: 0.8rem 0px;
  color: rgb(103, 113, 133);
}
.row .right > a,
.row .left > a,
.row .right > var,
.row .left > var,
.row .right > em,
.row .right > time,
.row .total {
  padding: 0.8rem 0px;
}
.row .right {
  justify-content: space-between;
}
.list ol {
  width: calc(100% - 100px);
  box-sizing: border-box;
  padding: 0.8rem 0px;
}
.list ol p {
  display: flex;
  justify-content: space-between;
  flex: 1;
}
.list ol a {
  overflow: hidden;
  text-overflow: ellipsis;
}
.list ol li var {
  flex-shrink: 1;
  margin-left: 10px;
}
.list > li .outnum {
  display: block;
  padding: 0.25rem;
  color: rgb(0, 135, 90);
  background: rgb(209, 240, 219);
  border: 1px solid rgb(209, 240, 219);
  border-radius: 0.25rem;
}
.row .right .title,
.row .inputs-view .title {
  opacity: 0;
}
.nothing {
  color: rgb(0, 135, 90);
}
.pager {
  display: flex;
  margin-bottom: 40px;
}
.pager span {
  width: 30px;
  height: 30px;
  color: #666;
  font-size: 14px;
  line-height: 30px;
  text-align: center;
  border: 1px solid #ccc;
  font-family: 'iconfont';
  cursor: pointer;
}
.pager form input {
  width: 80px;
  height: 30px;
  border: none;
  border-top: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
  outline: none;
  text-align: center;
}
.pager form button {
  border: none;
  width: 50px;
  height: 32px;
  background: rgb(12, 108, 242);
  color: #fff;
  cursor: pointer;
}

@media screen and (max-width: 500px) {
  .list > li .row {
    flex-direction: column-reverse;
  }
  .list > li .row .left,
  .list > li .row .right {
    width: 100%;
    justify-content: flex-start;
  }
  .list ol p {
    flex-direction: column;
  }
  .list ol li var {
    margin: 0;
  }
  .row .right .title,
  .row .inputs-view .title {
    opacity: 1;
  }
  .pager {
    display: flex;
    justify-content: center;
  }
  .pager span {
    width: 50px;
    height: 50px;
    line-height: 50px;
    font-size: 18px;
  }
  .pager form input {
    height: 50px;
    width: 140px;
  }
  .pager form button {
    height: 52px;
  }
}
</style>
