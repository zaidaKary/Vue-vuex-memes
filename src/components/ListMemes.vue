<template>
  <h1>{{ title }}</h1>
  <div class="total-show">
    <select class="form-select form-select-lg mb-3" @change="changeTotalShow">
      <option value="">All</option>
      <option value="5">5</option>
      <option value="10">10</option>
      <option value="20">20</option>
    </select>
  </div>
  <div class="row">
    <template v-for="meme in memes" :key="meme.id">
      <Meme :meme="meme" />
    </template>
  </div>
</template>

<script>
import { onMounted, computed } from "vue";
import { useStore } from "vuex";
import Meme from "./Meme";

export default {
  components: {
    Meme,
  },
  setup() {
    const store = useStore();
    const memes = computed(() => store.state.memes);

    onMounted(() => {
      store.dispatch("getMemes");
    });

    const changeTotalShow = (e) => {
      store.dispatch("getMemes", {
        total: e.target.value,
      });
    };

    return {
      title: store.state.titleApp,
      memes,
      changeTotalShow,
    };
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}
.total-show {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 5px;
}
.total-show select {
  width: 50%;
}
</style>
