<template>
  <b-card no-body>
    <b-card-header header-tag="nav">
      <b-tabs content-class="mt-3">
        <!-- Nasabah Akad -->
        <b-tab title="Customer Pawn">
          <CustomerPawn />
        </b-tab>
        <!-- Akad Jatuh Tempo -->
        <b-tab title="Maturity of Contract" active>
          <MaturityContract />
        </b-tab>
        <!-- Pelunasan dan Lelang -->
        <b-tab title="Settlement & Auction">
          <SettlementAuction />
        </b-tab>
      </b-tabs>
    </b-card-header>
  </b-card>
</template>

<script>
import CustomerPawn from "@/modules/contractData/CustomerPawn";
import MaturityContract from "@/modules/contractData/MaturityContract";
import SettlementAuction from "@/modules/contractData/SettlementAuction";

export default {
  data() {
    return {
      title: "Data Contract",
    };
  },
  components: {
    CustomerPawn,
    MaturityContract,
    SettlementAuction,
  },
};
</script>

<style lang="scss" scoped></style>
