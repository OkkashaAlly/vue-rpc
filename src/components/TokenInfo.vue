<template>
  <div>
    <h1>Token Symbol: {{ tokenSymbol }}</h1>
    <p>Wallet(address) Balance: {{ tokenBalance }}</p>
  </div>
</template>

<script>
import axios from 'axios'
import Web3 from 'web3'

export default {
  data() {
    return {
      web3: null,
      tokenAddress: '0x210E69a578CfCDbB7A829C7c6379Ac29E64A357a',
      tokenAbi: null,
      tokenSymbol: '',
      tokenBalance: ''
    }
  },
  created() {
    this.initWeb3()
  },
  methods: {
    async initWeb3() {
      // Initialize Web3 with provider
      if (window.ethereum) {
        this.web3 = new Web3(window.ethereum)
        await window.ethereum.enable()
      } else if (window.web3) {
        this.web3 = new Web3(window.web3.currentProvider)
      } else {
        console.log('No Web3 provider detected')
      }

      // Get token ABI - from polygonscan
      const apiUrl = `https://api.polygonscan.com/api?module=contract&action=getabi&address=${this.tokenAddress}`
      const response = await axios.get(apiUrl)
      this.tokenAbi = JSON.parse(response.data.result)

      // getting wallet address 
      const accounts = await window.ethereum.request({ method: 'eth_accounts' })

      // Get token symbol and balance
      const tokenContract = new this.web3.eth.Contract(this.tokenAbi, this.tokenAddress)
      const tokenSymbol = await tokenContract.methods.symbol().call()
      const tokenBalance = await tokenContract.methods.balanceOf(accounts[0]).call()

      console.log('tokenContract ', accounts[0])
      this.tokenSymbol = tokenSymbol
      this.tokenBalance = tokenBalance
    }
  }
}
</script>
