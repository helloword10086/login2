<template>
<div class="page-login">
  
    <div class="title">
       <h1>登陆</h1>
    </div>
    <div class="input">
          <van-cell-group>
                <van-field
              center
              clearable
              v-model="mycall"
              label="手机号"
              placeholder="请输入手机号"
             @input='myphone'
            />
            <van-field
              v-model="sms"
              center
              clearable
              label="短信验证码"
              placeholder="请输入短信验证码"
              @input="password"
            >
            </van-field>
          
        </van-cell-group>
        <div class="yan" v-if="msg" @click="getacha">获取验证码</div>
        <div class="yan" v-if='showcaha'>{{s}}后重置</div>
    </div>
     <div class="login" :class="phone"  v-if="flag"> 登录</div>
      <div class="login" :class="phone"  v-else @click="goindex"> 登录</div>
</div>
</template>

<script>
export default {
  name:'login' ,
  data() {
    return {
      msg:false,
      phone:'',
      sms:'',
      mycall:'',
      flag:true,
      s:60,
      showcaha:false
    }
  },
  computed: {
    // flag(){
    //   return this.msg
    // }
  },
  mounted() {
    // console.log(this.$route.query)
  },
  methods: {
    myphone(e){
      console.log(e)
      this.msg=false
      this.mycall=e
      if(/^1[3456789]\d{9}$/.test(e)){
        // console.log('111')
          this.msg=true;
          
          console.log(this.msg)
      }
    },
    password(e){
      
      console.log(e)
      
      if(e){
        this.phone='red',
        this.flag= false
      }else{
        this.phone='',
        this.flag= true
      }
      

    },
   getacha(){
     this.showcaha=true
     this.msg=false
     var id=  setInterval(() =>{
        this.s=this.s-1
      if(this.s===0){
        this.s=60
        clearInterval(id)
        this.showcaha = false;
        this.msg = true
      }
     },1000)

     this.axios.get('https://easy-mock.com/mock/5b2385e3debe3c5977248a16/wscn/captcha',{
       phone:this.mycall
     }).then(res =>{
       
       console.log(res)
       this.captcha=res.data.data.captcha
       this.$notify({
         message: `验证码为${res.data.data.captcha}`,
         duration: 3000,
          background: '#1989fa'
          } );

     })
   },
   goindex(){
      this.axios.post('https://easy-mock.com/mock/5b2385e3debe3c5977248a16/wscn/submit',{
        phone:this.mycall,
        captcha:this.captcha
      }).then((res) =>{
          this.$notify({
         message: `${res.data.data.messgae}`,
         duration: 3000,
          background: '#1989fa'
          } );
      }).catch((err) =>{
        this.$notify({
         message: `${err}`,
         duration: 3000,
          background: '#1989fa'
          } );
      })
   }
  },
}
</script>

<style scoped>
.page-login{
  background: #F4F7F9;
  width: 30%;
  margin: 0 auto;
  padding-bottom: 30px;
}
.title{
  margin-top: 90px;
  
}
.title h1{
  text-align: center;
  display: block;
  
}
.input{
  position: relative;
  width: 100%;
}
.yan{
  color: greenyellow;
  position: absolute;
  top: 54px;
  right: 35px;
}
.login{
  background: #E6E6E6;
  color: #fff;
  padding: 10px;
  text-align: center;
  border-radius: 10px;
  width: 80%;
  margin: 47px auto;
 /* margin-bottom: 30px */
}
.red{
  background: red
}
</style>
