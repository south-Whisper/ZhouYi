<template>
  <div class="input-panel">

    <br>

    <div>
      <label for="birthDate">请输入您的出生日期：</label>
      <input type="datetime-local" v-model="birthDate" />
    </div>

    <br>
    <br>

    <button @click="lunarCal">算卦</button>

    <div class="result" v-if="result">
      <p id="guaText">{{ result }}</p>
    </div>
  </div>
</template>

<script setup>
import { Solar } from 'lunar-javascript';
import { ref } from 'vue'
const birthDate = ref('')
const result = ref('')

//阴历ab算法
function lunarCal(){
  if(!birthDate.value){
    alert('请输入出生日期和问题')
    return
  }

  const date = new Date(birthDate.value);
  const solar = Solar.fromYmdHms(
      date.getFullYear(),
      date.getMonth() + 1,
      date.getDate(),
      date.getHours(),
      date.getMinutes(),
      date.getSeconds()
  );

  const lunar = solar.getLunar();
  const lunarYear = lunar.getYear();     // 农历年（数字）
  const lunarMonth = lunar.getMonth();   // 农历月（数字）
  const lunarDay = lunar.getDay();       // 农历日（数字）
  const hour = date.getHours();          // 使用阳历的小时

  // 3. 计算 a 和 b
  const a = (lunarYear + lunarMonth + lunarDay) % 8;
  const b = (lunarYear + lunarMonth + lunarDay + hour) % 8;

  const idx = (8 - a) % 8 + (8 - b) % 8 * 8;

  const gua = [
     2, 23,  8, 20, 16, 35, 45, 12,
    15, 52, 39, 53, 62, 56, 31, 33,
     7,  4, 29, 59, 40, 64, 47,  6,
    46, 18, 48, 57, 32, 50, 28, 44,
    24, 27,  3, 42, 51, 21, 17, 25,
    36, 22, 63, 37, 55, 30, 49, 13,
    19, 41, 60, 61, 54, 38, 58, 10,
    11, 26,  5,  9, 34, 14, 43,  1
  ]

  console.log(a + ' ' + ' ' + b + ' ' + gua[idx]);


}

// 获取卦象含义
function getGuaMeaning(index) {
  const guaMeanings = {
    1: "乾卦：乾为天，元亨，利贞。",
    2: "坤卦：坤为地，元亨，利牝马之贞。",
    3: "屯卦：水雷屯，元亨，利贞。",
    4: "蒙卦：山水蒙，亨。",
    5: "需卦：水天需，需有孚，光亨贞吉。",
    6: "讼卦：天水讼，有孚，窒惕，中吉。",
    7: "师卦：地水师，贞丈人吉，无咎。",
    8: "比卦：水地比，比吉。",
    9: "小畜卦：风天小畜，亨。",
    10: "履卦：天泽履，虎尾，不咥人，亨。",
    11: "泰卦：地天泰，小往大来，吉亨。",
    12: "否卦：天地否，不利君子贞。",
    13: "同人卦：天火同人，元亨。",
    14: "大有卦：火天大有，利贞。",
    15: "谦卦：地山谦，亨。",
    16: "豫卦：雷地豫，利贞。",
    17: "随卦：泽雷随，元亨。",
    18: "蛊卦：山风蛊，元亨，利贞。",
    19: "临卦：地泽临，元亨。",
    20: "观卦：风地观，有孚，众吉。",
    21: "噬嗑卦：火雷噬嗑，亨，利贞。",
    22: "贲卦：山火贲，亨，利贞。",
    23: "剥卦：山地剥，元亨。",
    24: "复卦：地雷复，元亨，利贞。",
    25: "无妄卦：天雷无妄，利贞。",
    26: "大蓄卦：山天大蓄，元亨。",
    27: "颐卦：山雷颐，贞吉。",
    28: "大过卦：泽风大过，利贞。",
    29: "坎卦：坎为水，习坎，入于坎窞，凶。",
    30: "离卦：离为火，丽火，亨。",
    31: "咸卦：泽山咸，利贞。",
    32: "恒卦：雷风恒，利贞。",
    33: "遯卦：天山遯，利贞。",
    34: "大壮卦：雷天大壮，利贞。",
    35: "晋卦：火地晋，利贞。",
    36: "明夷卦：地火明夷，利贞。",
    37: "家人卦：风火家人，利贞。",
    38: "睽卦：火泽睽，利贞。",
    39: "蹇卦：水山蹇，利贞。",
    40: "解卦：雷水解，利贞。",
    41: "损卦：山泽损，利贞。",
    42: "益卦：风雷益，利贞。",
    43: "夬卦：泽天夬，利贞。",
    44: "姤卦：天风姤，利贞。",
    45: "萃卦：泽地萃，利贞。",
    46: "升卦：地风升，利贞。",
    47: "困卦：泽水困，利贞。",
    48: "井卦：水风井，利贞。",
    49: "鼎卦：火风鼎，利贞。",
    50: "归妹卦：雷泽归妹，利贞。",
    51: "丰卦：雷火丰，利贞。",
    52: "旅卦：火山旅，利贞。",
    53: "巽卦：风为木，利贞。",
    54: "兑卦：泽为口，利贞。",
    55: "中孚卦：风泽中孚，利贞。",
    56: "小过卦：雷山小过，利贞。",
    57: "既济卦：水火既济，利贞。",
    58: "未济卦：火水未济，利贞。",
    59: "涣卦：风水涣，利贞。",
    60: "节卦：水泽节，利贞。",
    61: "兑卦：泽为口，利贞。",
    62: "巽卦：风为木，利贞。",
    63: "讼卦：天水讼，有孚，吉。",
    64: "困卦：泽水困，亨，利贞。",
  }

  return guaMeanings[(index % 64)]
}
</script>

<style scoped>

.input-panel {
  position: absolute;
  top: 325px; /* 距离页面顶端的距离，根据匣子位置微调 */
  left: 50%;
  width: 600px;
  transform: translateX(-50%);
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  opacity: 0;
  animation: fadeIn 2s ease forwards;
  animation-delay: 1s;
  z-index: 3; /* 确保在 box 后面 */
}


@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.result {
  margin-top: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  border: 1px solid #ddd;
  animation: slideDown 1s ease forwards;
}
@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

</style>