<template>
  <div class="last-transactions">
    <div class="block-body">
      <!-- Table Header -->
      <div class="tx-table-header" v-if="showHeader === true">
        <li>{{ $t('tableHeader.txN') }} </li>
        <li class="eth">{{ $t('common.eth') }}</li>
        <li class="limit">{{ $t( 'gas.limit' ) }}</li>
        <li class="gas">{{ $t( 'common.gwei' ) }}</li>
        <li></li>
        <!-- End Table Header -->
      </div>
      <div class="block" v-for="tx in transactions" v-bind:key="tx.getHash().toString()">
        <li>
          <div class="hash-block">
            <p class="hash"><router-link :to="'/tx/'+tx.getHash().toString()">{{tx.getHash().toString()}}</router-link></p>
          </div>
          <div class="fromto">
            <p class="title">{{ $t( 'tx.from' ) }}</p>
            <p class=""><router-link :to="'/address/'+tx.getFrom().toString()">{{tx.getFrom().toString()}}</router-link></p>

            <p class="title">{{ $t( 'tx.to' ) }}</p>
            <p class=""><router-link :to="'/address/'+tx.getTo().toString()">{{tx.getTo().toString()}}</router-link></p>
          </div>
        </li>
        <li class="vertical-middle eth"><div><p>{{tx.getValue().toEth()}}</p></div></li>
        <li class="vertical-middle gas"><div><p>{{tx.getGasUsed().toNumber()}}</p></div></li>
        <li class="vertical-middle"><div><p>{{tx.getGasPrice().toGWei()}}</p></div></li>
        <li class="vertical-middle status">
          <div v-if="!tx.getStatus()">
            <span class="glyphicon glyphicon-remove failed"></span>
          </div>
          <div v-else>
            <span class="glyphicon glyphicon-ok success"></span>
          </div>
        </li>
      </div>
    </div>

  </div>

</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'TableTransactions',
  props: ['transactions', 'showHeader'],
  methods: {
    /* Method to reduce Strig length : */
    getShortEthValue(newEthValue, isBool) {
      const length = newEthValue.length
      let isShort = false
      if (length > 6) {
        newEthValue = newEthValue.slice(0, 6) + '...'
        isShort = true
      }
      if (!isBool) {
        return newEthValue
      }
      return isShort
    }
  }
})
</script>

<style scoped lang="less">
@import '~lessPath/sunil/blocks/largeBlocks/transactionsTable.less';
</style>
