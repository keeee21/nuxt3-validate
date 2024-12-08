<script setup lang="ts">
import { ref } from 'vue';

// フォームデータ
const formData = ref({
  textField: '',
  radioOption: '',
  dropdown: '',
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

  <!-- テキスト入力 -->
  <!-- 文字数カウントする -->
  <div>
    <label for="textField">テキスト入力:</label>
    <input 
      type="text" 
      id="textField" 
      v-model="formData.textField" 
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
        v-model="formData.radioOption"
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
      v-model="formData.dropdown"
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
        v-model="formData.checkbox" 
      />
      チェックを入れてください
    </label>
  </div>
  <br>

  <!-- 送信ボタン -->
  <button type="submit">送信</button>
</template>