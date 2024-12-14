<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue';
import dropin from "braintree-web-drop-in"
import Swal from "sweetalert2"

let instance
// const payment_dropin = ref()
const handleSubmit = (evt) => {
  if(instance) {
    instance.requestPaymentMethod( async(err, payload) => {
      if(payload) {
        const { nonce } = payload
        const url = "https://newebpay-delta.vercel.app/"
        const { data } = await axios.post(url, { 
          order_number: 123, 
          payment_nonce: nonce
        })
        console.log(data);
        
        //如果有拿到資料就要打後端API

      }
    })
  }

}

onMounted(async() => {
  const url = "https://newebpay-delta.vercel.app/"
  const { data } = await axios.get(url)

  try {
    instance = await dropin.create({
    authorization: data.token,
    selector: "#dropin"
  })  
  } catch (error) {
    Swal.fire({
      title: 'Error!',
      text: '刷卡出錯，請稍後再試',
      icon: 'error',
      confirmButtonText: '好吧'
})
  }
})
  
  

</script>

<template>
  <h1>付款</h1>
  <div id="dropin"></div>
  <a herf="#" @click.prevent="handleSubmit">刷卡</a>
  <!-- 如果是btn就不需要prevent -->
</template>

<style scoped>

</style>