<script>
export default {
  methods: {
    addPolygonNetwork() {
      const networkName = 'Polygon'
      const chainId = 137
      const networkId = 137
      const rpcUrl = 'https://rpc-mainnet.maticvigil.com/'

      const provider = window.ethereum

      provider
        .request({
          method: 'wallet_addEthereumChain',
          params: [
            {
              chainId: `0x${chainId.toString(16)}`,
              chainName: networkName,
              nativeCurrency: {
                name: 'Matic',
                symbol: 'MATIC',
                decimals: 18
              },
              rpcUrls: [rpcUrl],
              blockExplorerUrls: [`https://polygonscan.com/`]
            }
          ]
        })
        .then(() => console.log('Polygon network added'))
        .catch((error) => console.error(error))
    },
    connect: function () {
      // this connects to the wallet

      if (window.ethereum) {
        // first we check if metamask is installed
        window.ethereum.request({ method: 'eth_requestAccounts' }).then(() => {
          this.connected = true // If users successfully connected their wallet
        })
      }
    }
  }
}
</script>

<template>
  <div>
    <button @click="addPolygonNetwork">Add Polygon Network</button>
  </div>
</template>

<style scoped></style>
