<template>
  <div class="row-item">
    <div class="row-item__group" :class="{ 'row-item__group_error': vld.$error }">
      <label class="row-item__label">{{ label }}{{ vld.$params.required ? ' *' : ''}}</label>
      {{ pref }}
      <input
        :class="vld.$error ? 'row-item__input row-item__input_error' : 'row-item__input'"
        v-model="value" @input="setValue($event.target.value, vld)"
        :style="`width:${width};`" />
    </div>
    <div class="row-item__error" v-if="vld.$params.required&&value.length&&!vld.required">
      Обязательное поле</div>
    <div class="row-item__error" v-else-if="customAlpha&&value.length&&!vld.customAlpha">
      Допускаются только буквы</div>
    <div class="row-item__error" v-else-if="customSpaceAlpha&&value.length&&!vld.customSpaceAlpha">
      Допускаются буквы и пробелы</div>
    <div class="row-item__error" v-else-if="vld.$params.numeric&&value.length&&!vld.numeric">
      Допускаются только цифры</div>
    <div class="row-item__error" v-else-if="vld.$params.minLength&&value.length&&!vld.minLength">
      Минимум знаков: {{vld.$params.minLength.min}}</div>
    <div class="row-item__error" v-else-if="vld.$params.maxLength&&value.length&&!vld.maxLength">
      Максимум знаков: {{vld.$params.maxLength.max}}</div>
  </div>
</template>

<script>
export default {
  name: "InputText",
  props: {
    label: String,
    vld: Object,
    val: Function,
    pref: {
      type: String,
      default: ''
    },
    width: {
      type: String,
      default: '10rem'
    }
  },
  data() {
    return {
      value: '',
      customAlpha: 'customAlpha' in this.vld.$params ? true : false,
      customSpaceAlpha: 'customSpaceAlpha' in this.vld.$params ? true : false
    }
  },
  methods: {
    setValue(value) {
      if (this.customAlpha || this.customSpaceAlpha) {
        this.value = value.toUpperCase()
      }
      this.val(this.value)
      this.vld.$touch()
    }
  }
}
</script>
