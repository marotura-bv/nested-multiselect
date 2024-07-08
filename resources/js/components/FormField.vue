<template>
  <DefaultField
    :field="field"
    :errors="errors"
    :show-help-text="showHelpText"
    :full-width-content="fullWidthContent"
  >
    <template #field>
      <Treeselect 
        :id="field.attribute"
        type="multiselect"
        v-model="selectedValues"
        :flat="field.flat"
        :content-type="'json'"
        :multiple="true"
        :options="options"
        :value-consists-of="'ALL_WITH_INDETERMINATE'"
        @input="handleInput"
      />
      <span v-if="errors.length" class="text-danger">{{ errors[0] }}</span>
    </template>
  </DefaultField>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
import { Treeselect } from 'vue3-treeselect'
import 'vue3-treeselect/dist/vue3-treeselect.css'

export default {
  components: { Treeselect },

  mixins: [FormField, HandlesValidationErrors],

  props: [
    'resourceName',
    'resourceId',
    'field',
    'errors',
  ],

  data() {
    return {
      options: [],
      selectedValues: [],
    }
  },

  methods: {
    /*
     * Set the initial, internal value for the field.
     */
    setInitialValue() {
      this.value = this.field.value || []
      this.options = JSON.parse(this.field.options[0].label);
      this.selectedValues = Array.isArray(this.value) ? this.value : JSON.parse(this.value);
    },

    /**
     * Fill the given FormData object with the field's internal value.
     */
    fill(formData) {
      let values = this.selectedValues.length > 0 ? JSON.stringify(this.selectedValues) : '';

      if (!this.field.required && values == '') {
        values = '[]';
      }

      formData.append(this.field.attribute, values);
    },

    /**
     * Handle the input event for the Treeselect component.
     */
    handleInput(values) {
      this.selectedValues = values;
      this.$emit('input', this.selectedValues);
    },
  },
}
</script>