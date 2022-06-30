<script>
	export default {
		onLaunch: function() {
		    console.log('App Launch');
		    // #ifdef APP-PLUS
		    // token标志来判断
		    let token= uni.getStorageSync('login_token');
				if (token) {
		        //不存在则跳转至登录页
		        plus.navigator.closeSplashscreen();
		    } else {
				//存在则关闭启动页进入首页
				uni.reLaunch({
				        url: "/pages/login/login",
				    success: () => {
				        plus.navigator.closeSplashscreen();
				    }
				})
		    }
		    // #endif
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>


<style lang="scss">
	/*每个页面公共css */
	@import "@/uni_modules/uview-ui/index.scss";
</style>