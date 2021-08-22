<template>
  <div>
    <div class="container mx-auto my-28 w-full max-w-xs">
      <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
            for="username"
          >
            バンド数を入力
          </label>
          <input
            v-model="bandCount"
            class="
              shadow
              appearance-none
              border
              rounded
              w-full
              py-2
              px-3
              text-gray-700
              leading-tight
              focus:outline-none
              focus:shadow-outline
            "
            type="number"
            placeholder="バンド数を入力"
          />
        </div>
        <div class="w-28 mx-auto flex items-center justify-between">
          <button
            @click="outputTimeTable({ bandCount })"
            class="
              bg-blue-500
              hover:bg-blue-700
              text-white
              font-bold
              py-2
              px-4
              rounded
              focus:outline-none
              focus:shadow-outline
            "
            type="button"
          >
            出力する
          </button>
        </div>
      </form>
    </div>
    <div class="text-center">
      <div class="font-bold text-xl text-green-900">タイムテーブル</div>
      <p>{{ timeTable }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bandCount: null,
      timeTable: "",
    }
  },
  methods: {
    outputTimeTable({ bandCount }) {
      this.timeTable = bandCount
    },
  },
}
</script>