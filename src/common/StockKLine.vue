<template>
  <div v-if="visible" class="modal-overlay" @click="closeModal">
    <div class="modal-content" @click.stop>
      <div class="buttons-container">
        <button
            :class="{ selected: period === 'day' }"
            @click="selectPeriod('day')"
        >
          日K
        </button>
        <button
            :class="{ selected: period === 'week' }"
            @click="selectPeriod('week')"
        >
          周K
        </button>
        <button
            :class="{ selected: period === 'month' }"
            @click="selectPeriod('month')"
        >
          月K
        </button>
        <button @click="adjustChart('lengthen')">拉长K线</button>
        <button @click="adjustChart('shorten')">缩短K线</button>
      </div>
      <img v-if="chartImage" :src="chartImage" alt="K线图"/>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      required: true,
    },
    stockCode: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      chartImage: null,
      unitWidth: -5,
      period: "day",
      type: "",
      stockCode: this.stockCode
    };
  },
  watch: {
    initialPeriod(newVal) {
      this.stockCode = newVal;
      this.fetchImage();
    },
    visible(newVal) {
      if (newVal && !this.chartImage) {
        this.fetchImage();
      }
    },
  },
  mounted() {
    // 在组件挂载后检查是否可见并获取图表
    if (this.visible && !this.chartImage) {
      this.fetchImage();
    }
  },
  methods: {
    closeModal() {
      this.$emit('update:visible', false);
    },
    selectPeriod(period) {
      if (period === 'week') {
        this.type = "W";
      } else if (period === 'month') {
        this.type = "M";
      } else {
        this.type = "";
      }
      this.period = period;
      this.fetchImage();
    },
    adjustChart(action) {
      // 这里可以根据action执行不同的操作
      console.log(`调整图表: ${action}`);
      if (action === 'lengthen') {
        this.unitWidth += 1;
      } else {
        this.unitWidth -= 1;
      }
      this.fetchImage();
    },
    // 获取图片
    async fetchImage() {
      try {
        this.cancelRequest() // 取消之前的请求

        this.loading = true
        const source = this.$axios.CancelToken.source()
        this.currentRequest = source
        let url =
            "https://webquoteklinepic.eastmoney.com/GetPic.aspx?nid=" + this.stockCode + "&type=" + this.type + "&unitWidth=" + this.unitWidth + "&ef=&formula=MACD&AT=1&imageType=KXL&timespan=" +
            new Date().getTime();
        const {data} = await this.$axios.get(url, {
          responseType: 'blob', // 关键：指定响应类型为二进制
          cancelToken: source.token
        })

        // 生成临时URL
        const blobUrl = URL.createObjectURL(data)
        this.chartImage = blobUrl
      } catch (error) {
        if (!this.$axios.isCancel(error)) {
          console.error('获取K线图失败:', error)
          // 这里可以添加错误提示
        }
      } finally {
        this.loading = false
        this.currentRequest = null
      }
    },
    // 取消当前请求
    cancelRequest() {
      if (this.currentRequest) {
        this.currentRequest.cancel('取消重复请求')
      }
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  width: 540px;
}

.buttons-container button {
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.buttons-container .selected {
  background-color: #007bff;
  color: white;
}
</style>