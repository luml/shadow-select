<template>
  <div class="home" :class="$style.eHistoryWrapper">
    <div :class="$style.xx"></div>
    <!-- 交易明细 -->
    <div :class="$style.selectWrapper">
      <div @click="toggleSelect" :class="$style.selectButton">
        <span>{{ getRecordType }}</span>
        <!-- <img v-if="isOpen" :src="" alt="">
        <img v-else :src="" alt=""> -->
      </div>
      <p :class="$style.limits">显示近3个月明细</p>
    </div>
    <!-- <div v-show="isOpen" :class="$style.shadow"> -->
    <div v-show="isOpen" :class="$style.optionWrapper">
      <button @click="[(recordType = '0')]">全部</button>
      <button @click="[(recordType = '1')]">转入</button>
      <button @click="[(recordType = '2')]">转出</button>
    </div>
    <!-- <div :class="$style.shadowWrapper"></div> -->
    <!-- </div> -->
    <div :class="$style.historyBlock">
      <!-- v-scroll="handleScroll" -->
      <ul>
        <li
          v-for="(item, index) in filterRecords"
          :key="index"
          :class="$style.historyItem"
        >
          <div>
            <p :class="$style.type">
              {{ getType(item.type) }} - {{ item.code }}
            </p>
            <p :class="$style.created">{{ item.createdTime }}</p>
          </div>
          <div>
            <p :class="$style.num">
              {{ item.type === "1" ? "+" : "-" }}{{ item.num }}
            </p>
            <p :class="$style.total">{{ item.total }}</p>
          </div>
        </li>
      </ul>
      <p v-if="showNoMore" :class="$style.ending">没有更多记录了</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
// import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

export default defineComponent({
  name: "Home",
  // directives: {
  //   scroll: {
  //     inserted: function (el, binding) {
  //       const f = function (evt) {
  //         if (binding.value(evt, el)) {
  //           window.removeEventListener('scroll', f)
  //         }
  //       }
  //       window.addEventListener('scroll', f)
  //     }
  //   }
  // },
  data() {
    return {
      isOpen: false,
      recordType: "0", // 全部
      showNoMore: false,
      historyRecords: [
        {
          type: "1", // '转入',
          code: "充值",
          num: 3000,
          total: 4000,
          createdTime: "2021-07-26 00:00:00",
        },
        {
          type: "2", // '转出',
          code: "充值",
          num: 3000,
          total: 4000,
          createdTime: "2021-07-26 00:00:00",
        },
        {
          type: "2", // '转出',
          code: "充值",
          num: 3000,
          total: 4000,
          createdTime: "2021-07-26 00:00:00",
        },
      ],
    };
  },
  computed: {
    getRecordType() {
      switch (this.recordType) {
        case "0":
          return "全部";
        case "1":
          return "转入";
        case "2":
          return "转出";
        default:
          return "全部";
      }
    },
    filterRecords() {
      if (this.recordType === "0") return this.historyRecords;
      return this.historyRecords.filter(
        (item) => item.type === this.recordType
      );
    },
  },
  created() {
    this.getRecords();
  },
  methods: {
    toggleSelect() {
      this.isOpen = !this.isOpen;
    },
    getType(type) {
      if (type === "1") return "转入";
      return "转出";
    },
    getRecords() {
      // TODO query params: pageNum, pageCount
      // TODO 交易类型(全部，转入，转出) 只用来筛选
      if (this.historyRecords.length < 30) this.showNoMore = true;
    },
    // handleScroll: function (evt, el) {
    //   if (el.scrollHeight - window.scrollY <= window.innerHeight) {
    //     // TODO get pagination data
    //     this.historyRecords = []
    //     if (this.historyRecords.length < 10) {
    //       this.showNoMore = true
    //     }
    //   }
    // }
  },
});
</script>
<style lang="scss" module>
// @import "~@/assets/css/config.less";
.xx {
  height: 100vh;
  background: transparent;
  z-index: 1000;
  position: fixed;
  top: 0;
}

.eHistoryWrapper {
  padding: 15px 0;
  height: 100vh;
  background: gray;
  z-index: 999;

  .selectWrapper {
    padding: 0 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    .selectButton {
      display: flex;
      align-items: center;
      width: 57px;
      height: 22px;
      background: #ffffff;
      border: 1px solid #e60027;
      border-radius: 2px;
      font-family: PingFangSC-Regular;
      span {
        color: #e60027;
        font-size: 11px;
      }
    }
    .limits {
      font-family: PingFangSC-Regular;
      font-size: 14px;
      color: #999999;
      text-align: right;
    }
  }

  // .shadow {
  //   position: fixed;
  //   width: 100vw;
  //   height: 100vh;
  //   // background: rgb(199, 194, 194);
  //   .shadowWrapper {
  //     height: calc(100vh - 100px);
  //     background: transparent;
  //   }
  // }
  .optionWrapper {
    position: fixed;
    width: 100vw;
    height: 100px;
    padding: 30px 0;
    display: flex;
    justify-content: space-around;
    background: #ffffff;

    button {
      width: 105px;
      height: 37px;
      border: 1px solid #eeeeee;
      border-radius: 10px;
      color: #171e24;

      &:focus {
        background: #e60027;
        color: #ffffff;
      }
    }
  }

  .historyBlock {
    margin-top: 13px;
    .historyItem.shadow {
      background: #868181;
    }
    ul {
      padding: 0;
    }
    .historyItem {
      border: 1px solid #eeeeee;
      display: flex;
      justify-content: space-between;
      padding: 17px;
      height: 75px;
      background: #ffffff;
      font-family: PingFangSC-Regular;

      .type,
      .num {
        font-size: 17px;
        color: #171e24;
        line-height: 21px;
        margin-bottom: 12px;
      }
      .created,
      .total {
        font-size: 14px;
        color: #999999;
        line-height: 17px;
        text-align: right;
      }
    }
  }
  .ending {
    font-family: PingFangSC-Regular;
    font-size: 14px;
    color: #999999;
    text-align: right;
    display: flex;
    justify-content: center;
    margin-top: 50px;
  }
}
</style>
