<script setup lang="ts">
import { ref } from 'vue';
import { string as yupString, object as yupObject, bool as yupBool } from 'yup';

const validationErrors = ref<string[]>([]);

const sampleData = ref({
  textField: '',
  radioOption: '',
  dropdown: '',
  checkbox: false,
});

const schema = yupObject().shape({
  textField: yupString().required('テキスト入力は必須です').max(5, '5文字以内で入力してください'),
  radioOption: yupString().required('ラジオボタンは必須です'),
  dropdown: yupString().required('ドロップダウンは必須です'),
  checkbox: yupBool().oneOf([true], 'チェックボックスは必須です'),
});

const saveAction = async () => {
  console.log(sampleData.value);
  try {
    //  { abortEarly: false } にすると、エラーが複数ある場合に全て表示する
    await schema.validate(sampleData.value, { abortEarly: false });
    window.alert('バリデーション成功');
  } catch (error) {
    validationErrors.value = error.errors;
    window.alert(error);
  }

}

// フォームデータ
const formData = ref({
  textField: null,
  radioOption: null,
  dropdown: null,
  checkbox: false,
});

// ラジオボタンのオプションデータ
const radioOptions = ref([
  { value: 'option1', label: 'オプション1' },
  { value: 'option2', label: 'オプション2' },
]);

// セレクトボックスのオプションデータ
const selectOptions = ref([
  { value: 'value1', label: '値1' },
  { value: 'value2', label: '値2' },
]);
</script>

<template>
  <h1>一般的なフォーム</h1>

  <div v-if="validationErrors.length > 0">
    <ul>
      <li v-for="error in validationErrors" :key="error">
        {{ error }}
      </li>
    </ul>
  </div>

  <!-- テキスト入力 -->
  <!-- 文字数カウントする -->
  <div>
    <label for="textField">テキスト入力:</label>
    <input 
      type="text" 
      id="textField"
      v-model="sampleData.textField" 
    />
  </div>
  <br>

  <!-- ラジオボタン -->
  <div>
    <p>ラジオボタン:</p>
    <label
      v-for="radioOption in radioOptions"
      :key="radioOption.value"
      :for="radioOption.label"
      >
      <input
        :id="radioOption.label" 
        type="radio" 
        :value="radioOption.value" 
        v-model="sampleData.radioOption"
      />
      {{ radioOption.label }}
    </label>
  </div>
  <br>

  <!-- セレクトボックス -->
  <div>
    <label for="dropdown">ドロップダウン:</label>
    <select 
      id="dropdown" 
      v-model="sampleData.dropdown"
    >
      <option disabled value="">選択してください</option>
      <option 
        v-for="option in selectOptions" 
        :key="option.value" 
        :value="option.value"
      >
        {{ option.label }}
      </option>
    </select>
  </div>
  <br>

  <!-- チェックボックス -->
  <div>
    <label>
      <input 
        type="checkbox" 
        v-model="sampleData.checkbox" 
      />
      チェックを入れてください
    </label>
  </div>
  <br>

  <!-- 送信ボタン -->
  <button type="button" @click="saveAction">送信</button>
</template>