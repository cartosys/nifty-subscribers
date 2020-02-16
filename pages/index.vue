<template>
  <div class="container">
    <div>
      <h1 class="title">
        Nifty Subscriptions
      </h1>
      <h2 class="subtitle">
        Hold a Non-Fungible Token (nft) that grants you membership
      </h2>

      <br>
      <br>
      <h2 class="subtitle">
        Your Balance: {{balance}}
      </h2>
      

      
        <button
          type="button"
          @click="joinRewards">
          Join Rewards
        </button>
          
      <div
        v-if="balance>0"
      >
        <br>
        <br>
        <h2 class="subtitle">
          Congratulations! You are now a member!
 
        </h2>
      </div>
      <div>
        <br>
        <button
          type="button"
          @click="buyNFT">
          Buy NFT
        </button>
      </div>  
    </div>
  </div>
</template>

<script>
import Web3 from 'web3'
import { ethers } from 'ethers'

export default {
  components: {},
  data() {
    return {
      web3js: null,
      ethereumProvider: null,
      ethereumAddress: null,
      ethereumToken: null,
      balance: 0,
      totalSupply: 0
    }
  },

  async mounted() {
    await this.initWeb3()
    await this.initContracts()
  },

  methods: {
    async joinRewards(){
      await this.ethereumToken.methods.mintSub(this.ethereumAddress).send({from: this.ethereumAddress }).then(response => {
          alert('Membership Token Received!')
          window.location.reload(true)
        })

      

      
    },
    async buyNFT(){
      if (this.balance>0){
        alert('Membership verified discount Applied')
      }else{
        alert('Membership not verified. No discount.')
      }
    },
    async initWeb3(){
      let web3js
      if (window.ethereum) {
        window.web3 = new Web3(ethereum)
        web3js = new Web3(ethereum)
        await ethereum.enable()
      } else if (window.web3) {
        window.web3 = new Web3(window.web3.currentProvider)
        web3js = new Web3(window.web3.currentProvider)
      } else {
        throw new Error('Please enable Metamask and refresh.')
      }
      var networkId = await web3js.eth.net.getId()

      if (web3js) {
        this.web3js = web3js
        this.ethereumProvider = new ethers.providers.Web3Provider(
          web3js.currentProvider
        )
        this.ethereumAddress = (await this.ethereumProvider.listAccounts())[0]
        window.ethereum.on('accountsChanged', function(accounts) {
          location.reload()
        })
      }
    },
    async initContracts() {
      const tokenABI = [{
          "inputs": [],
          "stateMutability": "nonpayable",
          "type": "constructor"
        },
        {
          "anonymous": false,
          "inputs": [{
              "indexed": true,
              "internalType": "address",
              "name": "owner",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "address",
              "name": "approved",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            }
          ],
          "name": "Approval",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [{
              "indexed": true,
              "internalType": "address",
              "name": "owner",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "address",
              "name": "operator",
              "type": "address"
            },
            {
              "indexed": false,
              "internalType": "bool",
              "name": "approved",
              "type": "bool"
            }
          ],
          "name": "ApprovalForAll",
          "type": "event"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "to",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            }
          ],
          "name": "approve",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "address",
            "name": "subscriber",
            "type": "address"
          }],
          "name": "mintSub",
          "outputs": [{
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }],
          "stateMutability": "payable",
          "type": "function"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "from",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "to",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            }
          ],
          "name": "safeTransferFrom",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "from",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "to",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            },
            {
              "internalType": "bytes",
              "name": "_data",
              "type": "bytes"
            }
          ],
          "name": "safeTransferFrom",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "operator",
              "type": "address"
            },
            {
              "internalType": "bool",
              "name": "approved",
              "type": "bool"
            }
          ],
          "name": "setApprovalForAll",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "uint256",
            "name": "newPrice",
            "type": "uint256"
          }],
          "name": "setPrice",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "anonymous": false,
          "inputs": [{
              "indexed": true,
              "internalType": "address",
              "name": "from",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "address",
              "name": "to",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            }
          ],
          "name": "Transfer",
          "type": "event"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "from",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "to",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "tokenId",
              "type": "uint256"
            }
          ],
          "name": "transferFrom",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "address",
            "name": "owner",
            "type": "address"
          }],
          "name": "balanceOf",
          "outputs": [{
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "baseURI",
          "outputs": [{
            "internalType": "string",
            "name": "",
            "type": "string"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "uint256",
            "name": "tokenId",
            "type": "uint256"
          }],
          "name": "getApproved",
          "outputs": [{
            "internalType": "address",
            "name": "",
            "type": "address"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "owner",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "operator",
              "type": "address"
            }
          ],
          "name": "isApprovedForAll",
          "outputs": [{
            "internalType": "bool",
            "name": "",
            "type": "bool"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "name",
          "outputs": [{
            "internalType": "string",
            "name": "",
            "type": "string"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "uint256",
            "name": "tokenId",
            "type": "uint256"
          }],
          "name": "ownerOf",
          "outputs": [{
            "internalType": "address",
            "name": "",
            "type": "address"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "bytes4",
            "name": "interfaceId",
            "type": "bytes4"
          }],
          "name": "supportsInterface",
          "outputs": [{
            "internalType": "bool",
            "name": "",
            "type": "bool"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "symbol",
          "outputs": [{
            "internalType": "string",
            "name": "",
            "type": "string"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "uint256",
            "name": "index",
            "type": "uint256"
          }],
          "name": "tokenByIndex",
          "outputs": [{
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
              "internalType": "address",
              "name": "owner",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "index",
              "type": "uint256"
            }
          ],
          "name": "tokenOfOwnerByIndex",
          "outputs": [{
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [{
            "internalType": "uint256",
            "name": "tokenId",
            "type": "uint256"
          }],
          "name": "tokenURI",
          "outputs": [{
            "internalType": "string",
            "name": "",
            "type": "string"
          }],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "totalSupply",
          "outputs": [{
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }],
          "stateMutability": "view",
          "type": "function"
        }
      ]
      //try {
        this.ethereumToken = new web3.eth.Contract(
          tokenABI, 
          '0x446e57b8584f3606737f187ab18a5cead037bbc4'
        )

        this.balance = await this.ethereumToken.methods.balanceOf(this.ethereumAddress).call()
        this.totalSupply = await this.ethereumToken.methods.totalSupply().call()

        /*const balance = await this.ethereumToken.balanceOf(
          this.ethereumAddress,
          { from: this.ethereumAddress }
        )*/


        
        //contract.methods.price.call().then(
        //  function (res) { console.log(res); })
        /*
        this.ethereumToken = new this.web3js.Contract(
          '0x446e57b8584f3606737f187ab18a5cead037bbc4',
          tokenABI,
          this.ethereumProvider.getSigner()
        )*/
        //call.mintSub(web3.provider.address, getPrice())
      //} catch (e) {
      //  console.log(e)
      //}
      
    },
  }
}

</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
