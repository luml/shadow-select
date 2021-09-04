<template>
  <div :class="$style.eHistoryWrapper">
    <!-- records -->
    <div :class="$style.selectWrapper">
      <div @click="toggleSelect" :class="$style.selectButton">
        <span>{{ getRecordType }}</span>
      </div>
      <p :class="$style.limits">显示近3个月明细</p>
    </div>
    <!-- <div v-show="isOpen" :class="$style.shadow"> -->
    <div v-show="isOpen" :class="$style.optionWrapper">
      <div id="option" :class="$style.option">
        <button @click="[(recordType = '0')]">全部</button>
        <button @click="[(recordType = '1')]">转入</button>
        <button @click="[(recordType = '2')]">转出</button>
      </div>
      <div :class="$style.restWrapper"></div>
    </div>
    <div :class="$style.historyBlock">
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
import gsap from "gsap";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Home",
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
  watch: {
    isOpen(newV, oldV) {
      if (newV && !oldV) {
        gsap.from("#option", {
          opacity: 0.2,
          y: -2,
          duration: .3,
        });
      }
    },
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
    // TODO handleScroll
    toggleSelect() {
      this.isOpen = !this.isOpen;
    },
    getType(type) {
      if (type === "1") return "转入";
      return "转出";
    },
    getRecords() {
      if (this.historyRecords.length < 30) this.showNoMore = true;
    },
  },
});
</script>
<style lang="scss" module>
.eHistoryWrapper {
  padding: 15px 0;
  height: 100vh;
  z-index: 999;

  .selectWrapper {
    padding: 0 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    > * {
      margin: 0;
    }
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

  .optionWrapper {
    .option {
      padding-top: 30px;
      height: 70px;
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
    .restWrapper {
      position: fixed;
      top: 130px;
      width: 100vw;
      height: calc(100vh - 100px);
      background: black;
      opacity: 0.4;
    }
    position: fixed;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: space-around;
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
