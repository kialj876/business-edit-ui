<template>
  <v-form lazy-validation>
    <v-layout row>
        <v-checkbox
          v-model="correctToNumbered"
          :label="`Change the company name to ${getBusinessId} B.C. Ltd.`"
        ></v-checkbox>
    </v-layout>
  </v-form>
</template>

<script lang="ts">
// Libraries
import { Component, Prop, Watch, Emit, Vue } from 'vue-property-decorator'
import { Action, Getter } from 'vuex-class'

// Interfaces && enums
import { ActionBindingIF } from '@/interfaces'
import { CorrectionTypes, EntityTypes } from '@/enums'

@Component({})
export default class CorrectNameToNumber extends Vue {
  /** Form Submission Prop */
  @Prop({ default: null }) formType: CorrectionTypes

  @Action setNameRequest!: ActionBindingIF

  @Getter getApprovedName!: string
  @Getter getEntityType!: EntityTypes
  @Getter getBusinessId!: string

  // Local Properties
  private correctToNumbered = false

  /** Watch for form submission and emit results. */
  @Watch('formType')
  private async onSubmit (): Promise<any> {
    if (this.formType === CorrectionTypes.CORRECT_NAME_TO_NUMBER) {
      const correctedNameToNumber = { legalType: this.getEntityType }
      this.setNameRequest(correctedNameToNumber)
      this.emitDone(true)
    }
  }

  /** Inform parent the process is complete. */
  @Emit('done')
  private emitDone (isSaved: boolean): void {}

  /** Inform parent when form is valid and ready for submission. */
  @Watch('correctToNumbered')
  @Emit('isValid')
  private emitValid (): boolean {
    return this.correctToNumbered
  }
}
</script>

<style lang="scss" scoped>
  .v-input--selection-controls {
    padding: 0;
    margin: 0;
  }
</style>
