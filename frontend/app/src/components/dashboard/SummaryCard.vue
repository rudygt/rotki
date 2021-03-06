<template>
  <v-card
    :loading="isLoading"
    :class="`dashboard__summary-card__${name}`"
    class="pb-3"
  >
    <v-row no-gutters class="pa-3 secondary--text summary-card__header">
      <v-toolbar-title class="font-weight-medium text-capitalize">
        {{ name }} balances
        <v-tooltip v-if="this.$slots.tooltip" bottom max-width="300px">
          <template #activator="{ on }">
            <v-icon
              small
              class="mb-3 ml-1 summary-card__header__tooltip"
              v-on="on"
            >
              fa fa-info-circle
            </v-icon>
          </template>
          <div>
            <slot name="tooltip" />
          </div>
        </v-tooltip>
      </v-toolbar-title>
      <v-spacer />
      <span>
        <v-tooltip v-if="canRefresh" bottom max-width="300px">
          <template #activator="{ on }">
            <v-btn
              icon
              x-small
              color="primary"
              class="summary-card__refresh-icon"
              @click="refresh(name)"
              v-on="on"
            >
              <v-icon color="primary">
                fa-refresh
              </v-icon>
            </v-btn>
          </template>
          Refresh {{ name }} balances
        </v-tooltip>
      </span>
    </v-row>
    <v-list>
      <slot />
    </v-list>
  </v-card>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({})
export default class SummaryCard extends Vue {
  @Prop({ required: true })
  name!: string;
  @Prop({ required: false, default: false })
  isLoading!: boolean;
  @Prop({ required: false, default: false, type: Boolean })
  canRefresh!: boolean;

  refresh(balanceSource: string) {
    this.$emit('refresh', balanceSource.toLowerCase());
  }
}
</script>

<style scoped lang="scss">
.summary-card {
  .v-list-item {
    font-size: 0.8em;
    max-height: 48px;
  }

  &__refresh-icon {
    filter: grayscale(100%);

    &:hover {
      filter: grayscale(0);
    }
  }

  &__header {
    &__tooltip {
      visibility: hidden;
    }

    &:hover {
      .summary-card {
        &__header {
          &__tooltip {
            visibility: visible;
          }
        }
      }
    }
  }
}
</style>
