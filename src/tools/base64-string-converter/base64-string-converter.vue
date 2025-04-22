<script setup lang="ts">
import { ref } from 'vue';
import { useCopy } from '@/composable/copy';
import { base64ToText, isValidBase64, textToBase64 } from '@/utils/base64';
import { withDefaultOnError } from '@/utils/defaults';

const encodeUrlSafe = useStorage('base64-string-converter--encode-url-safe', false);
const decodeUrlSafe = useStorage('base64-string-converter--decode-url-safe', false);

const textInput = ref('');
const base64Output = computed(() => textToBase64(textInput.value, { makeUrlSafe: encodeUrlSafe.value }));
const { copy: copyTextBase64 } = useCopy({ source: base64Output, text: 'Base64 string copied to the clipboard' });

// const base64Input = ref('');
const textOutput = computed(() =>
  // withDefaultOnError(() => base64ToText(base64Input.value.trim(), { makeUrlSafe: decodeUrlSafe.value }), ''),
  withDefaultOnError(() => base64ToText(textInput.value.trim(), { makeUrlSafe: decodeUrlSafe.value }), ''),
);
const { copy: copyText } = useCopy({ source: textOutput, text: 'String copied to the clipboard' });
const b64ValidationRules = [
  {
    message: 'Invalid base64 string',
    validator: (value: string) => isValidBase64(value.trim(), { makeUrlSafe: decodeUrlSafe.value }),
  },
];
const b64ValidationWatch = [decodeUrlSafe];

const checkedValueRef = ref('Decode');
const disabled = ref(true);

console.log('loaded', checkedValueRef.value);

const handleChange = (e: Event) => {
  checkedValueRef.value = (e.target as HTMLInputElement).value;
  console.log('handleChange', checkedValueRef.value);
};
</script>

<template>
  <c-card>
    <n-form-item label="Enable URL safe" label-placement="left">
      <n-switch v-model:value="encodeUrlSafe" />
    </n-form-item>
    <c-input-text
      v-model:value="textInput"
      multiline
      placeholder="Please type or paste in the characters to be Base64 encoded or decoded here..."
      rows="6"
      label="String to encode or decode"
      raw-text
      mb-5
      :validation-rules="checkedValueRef === 'Decode' ? b64ValidationRules : []"
      :validation-watch="checkedValueRef === 'Decode' ? b64ValidationWatch : []"
    />

    <div flex justify-start mb-5 gap-2>
      <n-space>
        <n-radio
          :checked="checkedValueRef === 'Decode'"
          value="Decode"
          name="basic-demo"
          @change="handleChange"
        >
          Decode
        </n-radio>
        <n-radio
          :checked="checkedValueRef === 'Encode'"
          value="Encode"
          name="basic-demo"
          @change="handleChange"
        >
          Encode
        </n-radio>
      </n-space>
    </div>

    <c-input-text
      label="Base64 encoded or decoded string"
      :value="checkedValueRef === 'Decode' ? textOutput : base64Output"
      multiline
      readonly
      placeholder="The base64 encoding or decoding of your string will be here"
      rows="6"
      mb-5
    />

    <div flex justify-start>
      <c-button @click="checkedValueRef === 'Decode' ? copyText() : copyTextBase64()">
        Copy the parsed string
      </c-button>
    </div>
  </c-card>

  <!-- Base64 to string 
  <c-card title="Base64 to string">
    <n-form-item label="Decode URL safe" label-placement="left">
      <n-switch v-model:value="decodeUrlSafe" />
    </n-form-item>
    <c-input-text
      v-model:value="base64Input"
      multiline
      placeholder="Your base64 string..."
      rows="5"
      :validation-rules="b64ValidationRules"
      :validation-watch="b64ValidationWatch"
      label="Base64 string to decode"
      mb-5
    />

    <c-input-text
      v-model:value="textOutput"
      label="Decoded string"
      placeholder="The decoded string will be here"
      multiline
      rows="5"
      readonly
      mb-5
    />

    <div flex justify-center>
      <c-button @click="copyText()">
        Copy decoded string
      </c-button>
    </div>
  </c-card>
  -->
</template>

<style lang="less" scoped>
.c-card {
  flex: 0 1 1024px;
}
</style>
