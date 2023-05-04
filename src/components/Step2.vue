<template>
  <div>
    <h2 class="font-cal text-lg">
      To get tokens CONST🪙 tokens from faucet🚰 click on the button below
    </h2>
    <p class="font-josefin">Click to get cokens from faucet</p>
    <Button
      :isDisabled="false"
      :isFilled="true"
      content="get tokens"
      :isComingSoon="false"
      color="black"
      class="float-right"
      @click="getTokensFromFaucet"
    />
  </div>
</template>

<script lang="ts">
import { computed, reactive } from "vue";
import Button from "./Button.vue";
import { useStore } from "@nanostores/vue";
import { isWallet, sharedConnect } from "../utils/wallet";
import { isWalletConnected } from "../state/walletState";
import { CONSTANTINE_INFO } from "../utils/constant";

export default {
  components: { Button },
  methods: {
    async getTokensFromFaucet() {
      if (!this.isWalletConnected) {
        return console.log("To get tokens you need to connect your wallet");
      }

      const offlineSigner = window.keplr?.getOfflineSigner(
        CONSTANTINE_INFO.chainId
      );
      if (!offlineSigner) {
        return console.error("Failed to create offline signer");
      }
      const accounts = await offlineSigner.getAccounts();

      console.log("Trying to get tokens from faucet");
      const myHeaders = new Headers();
      myHeaders.append("accept", "application/json");
      myHeaders.append("Content-Type", "application/json");

      const raw = JSON.stringify({
        address: accounts[0].address,
        coins: ["5000000uconst"],
      });

      fetch("https://faucet.constantine-2.archway.tech/", {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
        mode: "no-cors",
      })
        .then((response) => response.json())
        .then(() => console.log("Succesfully got tokens from faucet"))
        .catch((error) => console.log("error", error.message));
    },
  },
  setup(props) {
    isWallet();
    const $collectionDescription = useStore(isWalletConnected);
    props = reactive(props);
    return {
      isWalletConnected: computed(() => $collectionDescription.value),
    };
  },
};
</script>
