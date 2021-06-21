<template>
  <div class="row-item">
    <div class="row-item__group" :class="{ 'row-item__group_error': vld.$error }">
      <label class="row-item__label">{{ label }}{{ vld.$params.required ? ' *' : ''}}</label>
      <select v-model="value" @change="setValue()"
        :multiple="mult" :size=" mult ? values.length : 1 "
        :class="vld.$error ? 'row-item__input row-item__input_error' : 'row-item__input'" >
        <option
          v-for="value in values"
          :key="value.index"
          >{{ value }}</option>
      </select>
    </div>
    <div class="row-item__error" v-if="vld.$params.required&&value.length&&!vld.required">
      Поле обязательное для заполнения</div>
  </div>
</template>

<script>
export default {
  name: "SelectText",
  props: {
    label: String,
    vld: Object,
    val: Function,
    values: Array,
    mult: Boolean
  },
  data() {
    return {
      value: []
    }
  },
  methods: {
    setValue() {
      this.val(this.value)
      this.vld.$touch()
    }
  }
}
</script>

<style lang="sass">
select
  width: 8rem

</style>