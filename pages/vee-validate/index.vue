<script setup lang="ts">
import { ref } from 'vue';
import { string as yupString, object as yupObject, bool as yupBool } from 'yup';
import { useField, useForm } from 'vee-validate';

const schemas = {
  textField: yupString().required('テキスト入力は必須です'),
  radioOption: yupString().required('ラジオボタンは必須です'),
  dropdown: yupString().required('ドロップダウンは必須です'),
  checkbox: yupBool().oneOf([true], 'チェックボックスは必須です'),
};

// vee-validateでフォーム全体を管理
const { handleSubmit } = useForm({
  validationSchema: yupObject(schemas),
});

// 各フィールドのバリデーション
// useField の第1引数は、Yupスキーマのキーと一致させる必要がある
const { value: formText, errorMessage: formTextError } = useField<string>('textField', schemas.textField);
const { value: radioOption, errorMessage: radioOptionError } = useField<string>('radioOption', schemas.radioOption);
const { value: dropdown, errorMessage: dropdownError } = useField<string>('dropdown', schemas.dropdown);
const { value: checkbox, errorMessage: checkboxError } = useField<boolean>('checkbox', schemas.checkbox, { initialValue: false });

// 送信ボタンがクリックされたときにフォームを送信する
const saveAction = handleSubmit((values) => {
  alert('フォームが送信されました!');
  console.log(values);
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

  <!-- エラーメッセージ表示 -->
  <div v-if="formTextError || radioOptionError || dropdownError || checkboxError">
    <ul>
      <li v-if="formTextError">{{ formTextError }}</li>
      <li v-if="radioOptionError">{{ radioOptionError }}</li>
      <li v-if="dropdownError">{{ dropdownError }}</li>
      <li v-if="checkboxError">{{ checkboxError }}</li>
    </ul>
  </div>

  <!-- テキスト入力 -->
  <div>
    <label for="textField">テキスト入力:</label>
    <input 
      type="text" 
      id="textField" 
      v-model="formText" 
    />
  </div>
  <br>

  <!-- ラジオボタン -->
  <div>
    <p>ラジオボタン:</p>
    <label
      v-for="radio in radioOptions"
      :key="radio.value"
      :for="radio.label"
    >
      <input
        :id="radio.label"
        type="radio"
        :value="radio.value"
        v-model="radioOption"
      />
      {{ radio.label }}
    </label>
  </div>
  <br>

  <!-- セレクトボックス -->
  <div>
    <label for="dropdown">ドロップダウン:</label>
    <select 
      id="dropdown" 
      v-model="dropdown"
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
        v-model="checkbox"
      />
      チェックを入れてください
    </label>
  </div>
  <br>

  <!-- 送信ボタン -->
  <button type="button" @click="saveAction">送信</button>
</template>