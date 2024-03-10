<template>
  <div
    class="flex flex-col w-full"
  >
    <label v-if="label" :for="id" class="font-bold block capitalize mb-1">
      {{ label }}
    </label>
    <input
      :id="id"
      :type="type"
      :value="modelValue"
      @input="updateInput"
      class="border border-amber-400 shadow-sm rounded-md w-full p-1 focus-within:ring-sky-500 focus-within:ring-[1px] focus-within:ring-offset-2 focus-within:border-sky-500 outline-none transition duration-300 ease-in-out caret-sky-400"
      :class="{ 'border-red-500': showError }"
    />
    <p v-if="showError" class="text-red-500 text-sm mt-1">Please add a {{ label }}</p>
  </div>
</template>

<script>
export default {
  name: "BasicInput",
  props: {
    id: {
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "",
    },
    modelValue: {
      type: [String, Number],
      default: "",
    },
    type: {
      type: String,
      default: "text",
    },
    showError: {
      type: Boolean,
      default: false,
    }
  },
  methods: {
    updateInput(event) {
      this.$emit("update:modelValue", event.target.value);
    }
  }
};
</script>