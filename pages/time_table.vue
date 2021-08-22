<template>
  <div>
    <div class="container mx-auto my-28 w-full max-w-xs">
      <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
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
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
          >
            リハーサル時間
          </label>
          <input
            v-model="rehearsalTime"
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
            placeholder="リハーサル時間を入力"
          />
        </div>
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
          >
            本番の時間
          </label>
          <input
            v-model="performanceTime"
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
            placeholder="本番の時間を入力"
          />
        </div>
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
          >
            ライブ開始時間
          </label>
          <div class="text-center">
            <input v-model="startTime" type="time" class="form-time">
          </div>
        </div>
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
          >
            換気の有無
          </label>
          <div class="text-center">
            <input v-model="ventilation" type="checkbox" class="form-checkbox">
          </div>
        </div>
        <div class="w-28 mx-auto flex items-center justify-between">
          <button
            @click="outputTimeTable({ bandCount, rehearsalTime, performanceTime, ventilation, startTime })"
            v-if="bandCount&&rehearsalTime&&performanceTime&&startTime"
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
      <p style="white-space: pre-wrap;" >{{ timeTable }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bandCount: null,
      timeTable: "",
      time: null,
      rehearsalTime: null,
      performanceTime: null,
      ventilation: false,
      startTime: null,
    }
  },
  methods: {
    outputTimeTable({ bandCount, rehearsalTime, performanceTime, ventilation, startTime }) {
      this.timeTable = "";
      // バンドの配列作成 ex) bandCountが5なら ["バンド1", "バンド2", "バンド3", "バンド4", "バンド5"]
      const bands = [...Array(Number(bandCount)).keys()].map(i => `バンド${++i}`);

      this.time = this.$moment(`2021-01-01T${startTime}:00`);
      this.rehearsal(bands, rehearsalTime, ventilation);
      this.performance_preparation();
      this.performance(bands, performanceTime, ventilation);
      return;
    },
    rehearsal(bands, rehearsalTime, ventilation) {
      // 最初にだけ音出しバンドが存在する
      this.timeTable += `${this.time.format('HH:mm')}〜${this.time.add(rehearsalTime, 'm').format('HH:mm')} 音出しバンド\n`;
      this.time.add(5, 'm'); 

      // 換気のために別でカウントする
      let isVentilationCount = 2;
      for (var i = bands.length - 1;i > -1;i--) {
        // タイムテーブルの肝の部分 ex) 15:00〜15:15 バンド1
        this.timeTable += `${this.time.format('HH:mm')}〜${this.time.add(rehearsalTime, 'm').format('HH:mm')} ${bands[i]}\n`;

        // 転換分で5分追加。顔合わせはすぐやるため最後のバンドの時は追加しない。
        if (ventilation && isVentilationCount % 3 == 0 && i !== 0) {
          this.timeTable += `${this.time.format('HH:mm')}〜${this.time.add(5, 'm').format('HH:mm')} <換気>\n`;
        } else if (i !== 0) {
         this.time.add(5, 'm'); 
        }
        isVentilationCount++
      }
      return;
    },
    performance_preparation() {
      this.timeTable += `${this.time.format('HH:mm')} ＼＼＼\\顔合わせ//／／／\n`
      this.timeTable += `START  [[[   ${this.time.add(30, 'm').format('HH:mm')}   ]]]\n`
      return;
    },
    performance(bands, performanceTime, ventilation) {
      // 換気のために別でカウントする
      let isVentilationCount = 1;
      for (var i = 0;i < bands.length;i++) {
        // タイムテーブルの肝の部分 ex) 15:00〜15:15 バンド1
        this.timeTable += `${this.time.format('HH:mm')}〜${this.time.add(performanceTime, 'm').format('HH:mm')} ${bands[i]}\n`;

        // 転換分で5分追加
        if (ventilation && isVentilationCount % 3 == 0 && i !== bands.length -1) {
          this.timeTable += `${this.time.format('HH:mm')}〜${this.time.add(5, 'm').format('HH:mm')} <換気>\n`;
        } else {
          this.time.add(5, 'm')
        }
        isVentilationCount++
      }
      return;
    }
  },
}
</script>