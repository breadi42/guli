<template>
  <div class="cart py-container">
    <!--主内容-->
    <div class="checkout py-container  pay">
      <div class="checkout-tit">
        <h4 class="fl tit-txt"><span class="success-icon"></span><span class="success-info">订单提交成功，请您及时付款！订单号：{{ pay.orderNo }}</span>
        </h4>
        <span class="fr"><em class="sui-lead">应付金额：</em><em class="orange money">￥{{ pay.totalFee }}</em></span>
        <div class="clearfix"></div>
      </div>
      <div class="checkout-steps">
        <div class="fl weixin">微信支付</div>
        <div class="fl sao">
          <p class="red">请使用微信扫一扫。</p>
          <div class="fl code">
            <!-- <img id="qrious" src="~/assets/img/erweima.png" alt=""> -->
            <!-- <qriously value="weixin://wxpay/bizpayurl?pr=R7tnDpZ" :size="338"/> -->
            <qriously :value=" pay.codeUrl " :size="338"/>
            <div class="saosao"></div>
          </div>
        </div>
        <div class="clearfix"></div>
        <!-- <p><a href="pay.html" target="_blank">> 其他支付方式</a></p> -->
      </div>
    </div>
  </div>
</template>

<script>
import orderAPI from '@/api/order'

export default {
  asyncData({params}) {
    return orderAPI.createNative(params.no).then(res => {
      return {
        pay: res.data
      }
    })
  },
  mounted() {
    let timer = setInterval(() => {
      orderAPI.queryOrderStatus(this.pay.orderNo).then(res => {
        if (res.code === 20000) {
          clearInterval(timer)
          this.$router.push({ path: '/course/' + this.pay.courseId })
        }
      })
    }, 3000)
  }
}
</script>