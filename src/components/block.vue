<!--
 * @Author: yaohui.hou
 * @Date: 2021-11-20 17:38:27
 * @LastEditTime: 2021-11-23 14:28:02
 * @Description: 项目描述
-->
<template>
  <div class="detail">
    <!-- 摘要 -->
    <ul class="summary">
      <li v-for="item in blockinfo" :key="item.key">
        <div class="title">
          <span>{{ item.key }}</span>
          <p class="point" v-if="item.point">
            <em>{{ item.point }}</em>
          </p>
        </div>
        <div class="attr">
          <span>{{ item.transform ? item.transform(blockJson[item.attr]) : blockJson[item.attr] }}</span>
        </div>
      </li>
    </ul>
    <h2>Block Transactions</h2>
    <!-- 交易列表 -->
    <transaction :transactionlist="blockJson.tx" />
  </div>
</template>

<script>
import { ref } from 'vue';
import transaction from './transaction.vue';
export default {
  props: ['blockJson'],
  components: {
    transaction,
  },
  setup() {
    const blockinfo = ref([
      {
        key: 'Hash',
        attr: 'hash',
        point: 'Unique identifier used to identify a particular block',
      },
      {
        key: 'Timestamp',
        attr: 'time',
        transform: function (t) {
          return new Date(parseInt(t) * 1000).toLocaleString().replace(/\//g, '-');
        },
      },
      {
        key: 'Height',
        attr: 'height',
        point: 'Number of blocks connected on the blockchain',
      },
      {
        key: 'Number of Transactions',
        attr: 'n_tx',
        point: 'Mathematical value of how hard it is to find a valid hash for this block',
      },
      {
        key: 'Bits',
        attr: 'bits',
        point: 'A sub-unit of BTC, equal to 0.000001 BTC',
      },
      {
        key: 'Weight',
        attr: 'weight',
        point:
          'A measurement to compare the size of different transactions to each other in proportion to the block size limit',
      },
      {
        key: 'Size',
        attr: 'size',
        point: 'Total size of the block',
      },
      {
        key: 'Nonce',
        attr: 'nonce',
        point: 'Random value that can be adjusted to satisfy the proof of work',
      },
      {
        key: 'Fee Reward',
        attr: 'fee',
        point: 'Amount of transaction fees rewarded to the miner for calculating the hash for this block',
        transform: function (n) {
          return (n / 100000000).toFixed(8) + 'BTC';
        },
      },
    ]);

    return {
      blockinfo,
    };
  },
};
</script>
<style scoped>
.detail {
  max-width: 980px;
  margin: 0 auto;
  overflow: hidden;
}
.detail h2 {
  padding: 1rem 0px;
  font-weight: 700;
  font-size: 1.5rem;
}
.summary {
  margin: 4rem auto 2rem;
}
.summary li {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
  border-bottom: 1px solid rgb(223, 227, 235);
}
.summary li .title {
  position: relative;
  width: 30%;
}
.summary li .attr {
  position: relative;
  flex-grow: 1;
  max-width: 70%;
}
.summary li .point {
  position: absolute;
  left: 40%;
  top: -20%;
  display: flex;
  width: 100rem;
  height: 3rem;
  transition: opacity 0.6s linear;
  opacity: 0;
}
.summary li .point em {
  line-height: 1.2rem;
  height: 1.2rem;
  padding: 0.4rem;
  border-radius: 0.2rem;
  background: rgba(0, 0, 0, 0.8);
  color: #fff;
  font-size: 1rem;
  font-style: normal;
}
.summary li span {
  display: block;
  width: 100%;
  height: auto;
  font-size: 0.95rem;
  padding: 0.8rem 0px;
}
.summary li .title span {
  color: rgb(103, 113, 133);
}
.summary li .title:hover .point {
  opacity: 1;
}
.summary li .attr span {
  color: rgb(53, 63, 82);
  overflow: hidden;
  text-overflow: ellipsis;
}
@media screen and (max-width: 500px) {
  .detail {
    width: 100%;
  }
}
</style>
