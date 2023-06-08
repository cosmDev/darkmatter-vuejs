<template>
  <div > 
         <v-card
        width="600" align="center" justify="center" 
         
          title="DarkMatter Wallet" 
          class="pa-4 mt-6 mx-auto"
        > 
      <v-img
        :width="150"
        aspect-ratio="16/9"
        cover
        src="https://i.imgur.com/TWCBMXG.png"
      ></v-img> 
      </v-card> 
  <v-sheet v-if="isLogged" width="600" color="#EEEEEE" class="mt-4 mx-auto">
     <v-row >
      <v-col> 
        <v-card class="accent">
          <v-card-title class="headline text-left">
            <v-col >
            Token
          </v-col>
          <v-col class="text-right">
 
              {{ walletAmount / 1000000 }} BCNA            
          </v-col>               
          </v-card-title> 
        </v-card>      
      </v-col>
      <v-col>
        <v-card class="accent">
          <v-card-title class="headline text-left">
            <v-col >
            Rewards
          </v-col>
          <v-col class="text-right">
              {{ rewardsAmount }} BCNA            
          </v-col>               
          </v-card-title> 
        </v-card>       
      </v-col>
    </v-row>  
  </v-sheet>
  <v-sheet width="600" class="mt-6 mx-auto">  
  
        <v-card
          v-if="isLogged"
          width="600"
          title="Send tx" 
          class="pa-4"
        >
        
        <v-form  validate-on="submit" @submit.prevent="submit">
          <v-text-field
            v-model="addressFrom" 
            label="Address From"
            variant="outlined"
          ></v-text-field>
          <v-text-field
            v-model="addressTo" 
            label="Address To"
            variant="outlined"
          ></v-text-field>
          <v-text-field
            v-model="amount" 
            label="Amount"
            variant="outlined"
          ></v-text-field>
          <v-btn block class="mt-2" @click="sendTx">Submit</v-btn>
          <v-btn block class="mt-2" @click="connect">Connect</v-btn>
        </v-form>
 
      </v-card>
      <v-form v-if="!isLogged && checkMatter" validate-on="submit" @submit.prevent="submit">
        <v-btn block class="mt-2" @click="connect">Connect</v-btn>
      </v-form> 
      <div v-if="!checkMatter" >
        <v-btn block class="mt-2" >Install darkMatter</v-btn>
      </div>       
  </v-sheet> 
<!--  <v-sheet width="600" class="mt-6 mx-auto">
    <iframe src="https://app.kado.money/?apiKey=3ed8eea0-4f0c-45ea-86c8-d48980aa8a29" width="500" height="700" style="border: 0px;"></iframe>
  </v-sheet>-->
  </div>
</template>

<script>
 
import { Tendermint34Client } from "@cosmjs/tendermint-rpc";
import { DarkMatter } from '@cosmdev/darkmatter-js';

const client = await Tendermint34Client.connect('https://rpc-testnet.bitcanna.io');
const darkmatter = new DarkMatter('bitcanna-1', client);
 


export default {
  name: 'HelloWorld',
  data: () => ({ 
    checkMatter: '',
    myAddress: '',
    addressFrom: '',
    addressTo: '',
    amount: '',
    isLogged: false,
    walletAmount: '',
    rewardsAmount: ''
  }), 
  async mounted () {
  
    let checkMatter = await darkmatter.initMatter()
    console.log(checkMatter)
    this.checkMatter = checkMatter
    /*setInterval(() => {
      this.getBlock()
    }, 5000)*/    
    
    
/*
const queryClient = new QueryClient(client);
const rpcClient = createProtobufRpcClient(queryClient);

// Here we instantiate a specific query client which will have the custom methods defined in the .proto file
const queryDistrib = new distrib.QueryClientImpl(rpcClient);
const queryBank = new bank.QueryClientImpl(rpcClient);
console.log(queryDistrib)

const queryDistribResult = await queryDistrib.DelegationTotalRewards({ delegatorAddress: 'bcna1sw8xa00s68szlyvgp8l2fzqj95w5gjm5auc3le'}); 
const queryBankResult = await queryBank.AllBalances({ address: 'bcna1sw8xa00s68szlyvgp8l2fzqj95w5gjm5auc3le' });
console.log(queryDistribResult.total[0].amount)   
console.log(queryBankResult)      */ 
  
    /*setInterval(async () => {
      const queryDistribResult = await queryDistrib.DelegationTotalRewards({ delegatorAddress: 'bcna1sw8xa00s68szlyvgp8l2fzqj95w5gjm5auc3le'});
      console.log(queryDistribResult.total[0].amount) 
    }, 1000) */  
  
  
  
  },
  methods: {
    sendTx () {
      darkmatter.sendToken(this.addressFrom, this.addressTo, this.amount, this.memo)
    }, 
    async connect () {
      // consuming the promise
      darkmatter.connect().then(async data => {
        console.log(data)
        this.myAddress = data
        this.addressFrom = data.data
        
        
        
        //let bank = setupBankExtension()
        //console.log(await bank.bank.allBalances(data.data))
        let walletAmount = await darkmatter.getBalance()
        let getRewardToken = await darkmatter.getRewardToken() 
        this.walletAmount = walletAmount 
        this.rewardsAmount = getRewardToken.toFixed(6)
        this.isLogged = true
        
        setInterval(async () => {
          let getRewardToken = await darkmatter.getRewardToken()
          this.rewardsAmount = getRewardToken.toFixed(6)
        }, 5000)        
             
      })
      .catch(err => {
        console.log(err)
      })      
      
    },     
    async getBlock () {
      console.log(await client.getBlock()) 
    }
  }   
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
