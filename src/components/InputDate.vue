<template>
  <div class="row-item">
    <div class="row-item__group" :class="{ 'row-item__group_error': vld.$error }">
      <label class="row-item__label">{{ label }}{{ vld.$params.required ? ' *' : ''}}</label>
      <input type="date"
        :class="vld.$error ? 'row-item__input row-item__input_error' : 'row-item__input'"
        v-model="value"
        @input="setValue($event.target.value)"
        :style="`width:${width};`" />
    </div>
    <div class="row-item__error" v-if="vld.$params.required&&value.length&&!vld.required">
      Введите дату
    </div>
  </div>
</template>


<script>
export default {
  name: "InputDate",
  props: {
    label: String,
    vld: Object,
    val: Function,
    width: {
      type: String,
      default: '11rem'
    }
  },
  data() {
    return {
      value: '',
    }
  },
  methods: {
    setValue(value) {
      this.val(value)
      console.log(this)
      this.vld.$touch()
    }
  }
}
</script>
