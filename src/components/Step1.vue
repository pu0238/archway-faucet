<template>
  <div>
    <h2 class="font-cal text-lg">
      To get tokens🪙 from faucet🚰 u need to connect🔗 your
      <a class="text-indigo-500" href="https://www.keplr.app/" target="_blank"
        >keplr wallet</a
      >👛 to this site. If you do not have keplr wallet install it on
      <a class="text-indigo-500" href="https://www.keplr.app/" target="_blank"
        >keplr website</a
      >🌐.
    </h2>
    <p class="font-josefin">Connect your wallet below</p>
    <Button
      :isDisabled="false"
      :isFilled="true"
      :content="isWalletConnected ? 'disconnect wallet' : 'connect wallet'"
      :isComingSoon="false"
      class="float-right"
      @click="sharedConnect"
    />
  </div>
</template>

<script lang="ts">
import { computed, reactive } from "vue";
import Button from "./Button.vue";
import { useStore } from "@nanostores/vue";
import { isWallet, sharedConnect } from "../utils/wallet";
import { isWalletConnected } from "../state/walletState";

export default {
  components: { Button },
  methods: {
    sharedConnect,
  },
  setup(props) {
    const $collectionDescription = useStore(isWalletConnected);
    props = reactive(props);
    return {
      isWalletConnected: computed(() => $collectionDescription.value),
    };
  },
};
</script>
