<template>
  <div>
    <input
      type="text"
      :value="searchTerm"
      @input="setSearchTerm"
      @blur="() => setListOpen(false)"
      @focus="() => setListOpen(true)"
    />
    <ul v-if="this.isFocus" id="list" @scroll="handleNotificationListScroll">
      <div class="options">
        <li v-if="dataList.length === 0">{{ errorMsg }}</li>
        <li
          v-else
          v-for="(item, index) in dataList"
          :value="index"
          :key="item.name"
          @mousedown="setSelected(item.name)"
        >
          {{ item.name }}
        </li>
      </div>
    </ul>
  </div>
</template>
<script>
/**
 * DropDown 
    @feature : 검색 및 스크롤 페이징 
 */
export default {
  name: "DropDown",
  props: {
    errorMsg: {
      type: String,
      default: null,
      required: false,
    },
    dataList: {
      type: Array,
      default: null,
      required: true,
    },
    searchTerm: {
      type: String,
      default: null,
      required: true,
    },
    total: {
      type: Number,
      default: null,
      required: true,
    },
  },
  data() {
    return {
      isFocus: false,
    };
  },
  methods: {
    setListOpen(isOpen) {
      this.isFocus = isOpen;
    },
    setSelected(obj) {
      this.$emit("setSelected", obj);
    },
    setSearchTerm(e) {
      this.$emit("setSearchTerm", e.target.value);
    },
    handleNotificationListScroll(e) {
      const { scrollHeight, scrollTop, clientHeight } = e.target;
      const isAtTheBottom =
        scrollHeight <= Math.round(scrollTop) + clientHeight;

      // 일정 한도 밑으로 내려오면 함수 실행
      if (isAtTheBottom) this.handleLoadMore();
    },
    handleLoadMore() {
      if (this.dataList.length < this.total) {
        this.$emit("handleLoadMore");
      }
    },
  },
};
</script>
<style scoped>
ul {
  list-style: none;
  height: 100px;
  overflow-x: hidden;
  overflow-y: scroll;
}
</style>
