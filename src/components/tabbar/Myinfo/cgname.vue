<template>
<div class="self-container">
	<div> 
      <header  id="header" class="mui-bar mui-bar-nav">
			<a @click="goback1" class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
            <button class="mui-pull-right butt" @click="changenick">更改</button>
			<h1 class="mui-title">设置昵称</h1>
	</header>      
    </div>
    <div  class="self-container">
			新昵称：<input type="text" v-model="nickname">
    </div>
</div>
</template>
<script>
import { Toast } from "mint-ui";
    export default {
		data(){
			return{
               nickname:""
			}
		},
        methods: {
             goback1:function(){
                this.$router.replace('/selfinfo')
                this.$emit('public_headern', true);
                this.$emit('public_footer', true);
            },
            changenick()
            {    
                console.log(this.nickname+"这是nickname")
                console.log(this.$store.state.user.name+"这是name！！！！")
                this.$http.post('http://47.103.14.235:27499/users/nickname', {
                     name:this.$store.state.user.name,
                     nickName:this.nickname
                }).then(result => {
                    console.log(JSON.stringify(result.data)+'这是返回！！！！')
                     Toast({
                                 message: '更改成功!',
                            });
                    this.$store.dispatch('login', result.data)
                        this.$router.replace('selfinfo')
                    })
           
        }
        },
        computed: {
            user () {
                return this.$store.state.user
            }
		},
		created(){
        
    }}

</script>
<style lang="scss" scoped>
.self-container{
    padding-top: 40px;
}
.butt{
    margin-top: 5px;
}
</style>
