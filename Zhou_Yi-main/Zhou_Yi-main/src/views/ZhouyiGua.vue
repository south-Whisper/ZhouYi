<template>
  <div class="container">
    <h1>周易算卦</h1>

    <div>
      <label for="birthDate">请输入您的出生日期：</label>
      <input type="date" v-model="birthDate" />
    </div>

    <div>
      <label for="question">请输入您的问题：</label>
      <textarea v-model="question" placeholder="请输入您的问题"></textarea>
    </div>

    <button @click="calculate">算卦</button>

    <div class="result" v-if="result">
      <p id="guaText">{{ result }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const birthDate = ref('')
const question = ref('')
const result = ref('')

function calculate() {
  if (!birthDate.value || !question.value.trim()) {
    alert('请输入出生日期和问题！')
    return
  }

  const seed = birthDate.value.replace(/-/g, '') + question.value.length
  const random = parseInt(seed) % 64
  result.value = `卦象为：${getGuaMeaning(random)}`
}

function getGuaMeaning(index) {
  const guaMeanings = {
    1: "乾卦：乾为天，元亨，利贞。",
    2: "坤卦：坤为地，元亨，利牝马之贞。",
    3: "屯卦：水雷屯，元亨，利贞。",
    4: "蒙卦：山水蒙，亨。匪我求童蒙，童蒙求我。",
    5: "需卦：水天需，需有孚，光亨贞吉。",
    6: "讼卦：天水讼，有孚，窒惕，中吉。",
    7: "师卦：地水师，贞丈人吉，无咎。",
    8: "比卦：水地比，比吉，原筮，元永贞，无咎。",
    9: "小畜卦：风天小畜，亨，密云不雨，自我西郊。",
    10: "履卦：天泽履，虎尾，不咥人，亨。",
    11: "泰卦：地天泰，小往大来，吉亨。",
    12: "否卦：天地否，不利君子贞，大往小来。",
    13: "同人卦：天火同人，元亨，利贞。",
    14: "大有卦：火天大有，利贞，亨。",
    15: "谦卦：地山谦，亨，君子有终。",
    16: "豫卦：雷地豫，利贞，亨。",
    17: "随卦：泽雷随，元亨，利贞。",
    18: "蛊卦：山风蛊，元亨，利贞。",
    19: "临卦：地泽临，元亨，利贞。",
    20: "观卦：风地观，盥而不荐，有孚，众吉。",
    21: "噬嗑卦：火雷噬嗑，亨，利贞。",
    22: "贲卦：山火贲，亨，利贞。",
    23: "剥卦：山地剥，元亨，利贞，亨。",
    24: "复卦：地雷复，元亨，利贞。",
    25: "无妄卦：天雷无妄，亨，利贞。",
    26: "大蓄卦：山天大蓄，元亨，利贞。",
    27: "颐卦：山雷颐，贞，吉，亨。",
    28: "大过卦：泽风大过，亨，利贞。",
    29: "坎卦：坎为水，习坎，入于坎窞，凶。",
    30: "离卦：离为火，丽火，亨。",
    31: "咸卦：泽山咸，亨，利贞。",
    32: "恒卦：雷风恒，亨，利贞。",
    33: "遯卦：天山遯，亨，利贞。",
    34: "大壮卦：雷天大壮，亨，利贞。",
    35: "晋卦：火地晋，亨，利贞。",
    36: "明夷卦：地火明夷，利贞。",
    37: "家人卦：风火家人，利贞。",
    38: "睽卦：火泽睽，利贞。",
    39: "蹇卦：水山蹇，利贞。",
    40: "解卦：雷水解，亨，利贞。",
    41: "损卦：山泽损，亨，利贞。",
    42: "益卦：风雷益，亨，利贞。",
    43: "夬卦：泽天夬，亨，利贞。",
    44: "姤卦：天风姤，亨，利贞。",
    45: "萃卦：泽地萃，亨，利贞。",
    46: "升卦：地风升，亨，利贞。",
    47: "困卦：泽水困，亨，利贞。",
    48: "井卦：水风井，亨，利贞。",
    49: "鼎卦：火风鼎，亨，利贞。",
    50: "大有卦：火天大有，亨，利贞。",
    51: "归妹卦：雷泽归妹，亨，利贞。",
    52: "无妄卦：风雷无妄，亨，利贞。",
    53: "升卦：地风升，亨，利贞。",
    54: "井卦：水风井，亨，利贞。",
    55: "小畜卦：风天小畜，亨，利贞。",
    56: "大有卦：火天大有，亨，利贞。",
    57: "大壮卦：雷山大壮，亨，利贞。",
    58: "革卦：泽火革，亨，利贞。",
    59: "升卦：地风升，亨，利贞。",
    60: "解卦：雷水解，亨，利贞。",
    61: "蛊卦：山风蛊，亨，利贞。",
    62: "家人卦：风火家人，利贞。",
    63: "萃卦：泽地萃，亨，利贞。",
    64: "蹇卦：水山蹇，利贞。",
  }

  return guaMeanings[(index % 64) + 1]
}
</script>

<style scoped>
.container {
  width: 80%;
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

label {
  font-size: 16px;
  margin: 10px 0;
  display: block;
}

input, textarea {
  width: 100%;
  padding: 8px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
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
}
</style>
