<template>
  <div>
    <div class="mt-10 benefit-company-statement" v-if="isTypeBcomp">
      <p>
        <span class="benefit-company-statement-label">{{ BenefitCompanyStatementResource.title }}:</span>
        {{ BenefitCompanyStatementResource.description }}
      </p>
    </div>

    <section class="mt-10">
      <header>
        <h2>Review</h2>
      </header>
      <Summary />
    </section>
    <section class="mt-10">
      <header>
        <h2>Incorporation Date and Time</h2>
        <div class="list-item">
          <p class="list-item__subtitle">Select the Date and Time of incorporation for your business. You may select a
            date up to 10 days in the future (note: there is an <strong>additional fee of $100</strong> to enter and
            incorporation date in the future). Unless a business has special requirements, most businesses select an
            immediate Date and Time of Incorporation.
          </p>
        </div>
      </header>
      <IncorporationDateTime
        :incorporationDateTime="incorporationDateTime"
        @valid="onValidDateTime($event)"
        @dateTime="setDateTime($event)"
      />
    </section>

    <section class="mt-10">
      <header>
        <h2>Completing Party Statement</h2>
      </header>
      <Certify
        :date="getCurrentDate"
        :certifiedBy="certifyState.certifiedBy"
        :certifyStatementResource="certifyStatementResource"
        @valid="onValid($event)"
        @certifiedBy="onCertifiedBy($event)"
      />
    </section>
  </div>
</template>

<script lang="ts">
// Libraries
import { Component, Mixins } from 'vue-property-decorator'
import { State, Action, Getter } from 'vuex-class'

// Interfaces
import { CertifyStatementIF, ActionBindingIF, CertifyIF, DateTimeIF, GetterIF } from '@/interfaces'

// Components
import { Certify, IncorporationDateTime, Summary } from '@/components/ReviewConfirm'

// Resources
import { BenefitCompanyStatementResource } from '@/resources'

@Component({
  components: {
    Certify,
    IncorporationDateTime,
    Summary
  }
})
export default class ReviewConfirm extends Mixins() {
  // Global state
  @State(state => state.stateModel.certifyState)
  readonly certifyState!: CertifyIF

  @State(state => state.resourceModel.certifyStatementResource)
  readonly certifyStatementResource!: CertifyStatementIF

  @State(state => state.stateModel.incorporationDateTime)
  readonly incorporationDateTime!: DateTimeIF

  // Global Getters
  @Getter isTypeBcomp!: GetterIF
  @Getter getCurrentDate!: string

  // Global setters
  @Action setIsIncorporationDateTimeValid!: ActionBindingIF
  @Action setEffectiveDate!: ActionBindingIF
  @Action setCertifyState!: ActionBindingIF

  // Resources for template
  readonly BenefitCompanyStatementResource = BenefitCompanyStatementResource

  /** Handler for Valid change event. */
  private onValid (val: boolean): void {
    this.setCertifyState(
      {
        valid: val,
        certifiedBy: this.certifyState.certifiedBy
      }
    )
  }

  /** Handler for CertifiedBy change event. */
  private onCertifiedBy (val: string): void {
    this.setCertifyState(
      {
        valid: this.certifyState.valid,
        certifiedBy: val
      }
    )
  }

  /** Handler for Valid DateTime change event. */
  private onValidDateTime (val: boolean): void {
    this.setIsIncorporationDateTimeValid(val)
  }

  /** Handler for setting DateTime. */
  private setDateTime (date: Date): void {
    this.setEffectiveDate(date)
  }
}
</script>

<style lang="scss" scoped>
.benefit-company-statement-label {
  letter-spacing: -0.04rem;
  font-weight: 700;
}
</style>
