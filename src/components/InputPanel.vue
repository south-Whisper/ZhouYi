<template>
  <div class="input-panel">
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
import { ref, defineProps } from 'vue'
const birthDate = ref('')
const question = ref('')
const result = ref('')

// 计算卦象
function calculate() {

  if (!birthDate.value || !question.value.trim()) {
    alert('请完善出生日期和问题')
    return
  }

  const yaos = []

  // 模拟六次铜钱投掷，得到六个爻（从下往上）
  for (let i = 0; i < 6; i++) {
    let sum = 0
    for (let j = 0; j < 3; j++) {
      sum += Math.random() < 0.5 ? 2 : 3 // 模拟抛硬币：正面=3，反面=2
    }
    const yao = sum % 2 === 0 ? 0 : 1 // 偶数为阴爻，奇数为阳爻
    yaos.push(yao)
  }

  // 得到下卦和上卦（三爻），顺序为从下到上
  const lower = yaos.slice(0, 3).reverse().join('')
  const upper = yaos.slice(3, 6).reverse().join('')

  // 八卦编号，转十进制
  const lowerIndex = parseInt(lower, 2)
  const upperIndex = parseInt(upper, 2)

  // 文王卦序中的编号（64卦中，八卦组合 index）
  const guaIndex = upperIndex * 8 + lowerIndex +1;

  result.value = `卦象为：${getGuaMeaning(guaIndex)}`
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