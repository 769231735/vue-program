<template>
	<div class="wrap">
		<div class="address-header">
			<span @click="go"><</span>
			<span class="s2">收货地址</span>
			<span><img src="../assets/3.png" alt=""></span>
		</div>
		<div class="main">
			<div class="con" v-for="(x,i) in address_list">
				<div class="top">
					<h4>{{x.name}}<span>{{x.phone}}</span></h4>
					<p>
                    <span>{{x.province}}</span>
                    <span>{{x.city}}</span>
                    <span>{{x.region}}</span>
                    <span>{{x.address}}</span>
                    </p>
				</div>
				<div class="bot">
					<p class="p1" @click="set_default(i)"><span class="circle" :class="{bg:x.is_default}"></span>设为默认</p>
					<p></p>
					<p class="p3">
						<img src="../assets/1.png" alt="">
						<span @click="edit_address(i)">编辑</span>
						<img src="../assets/1.png" alt="">
						<span @click="delete_address(i)">删除</span>
					</p>
				</div>	
			</div>	
    
				
			
			<p @click="add_address" class="add">
				<span class="s1">+</span>
				<span>新增加地址</span>
			</p>
		</div>
	</div>
	
</template>
<script>
import mapMutations from 'vuex';

	export default{
		name:"address_info",
		data(){
		  return {
              site:null,
              nav_dom:'',
              address_list:[]
		  }
        },
        created(){
            this.$http.get('/user/Address/index').then((data)=>{
                console.log(data)
            })

            if(localStorage.getItem('address')){
                this.$store.commit('add_address_info',JSON.parse(localStorage.getItem('address')))
            }
            
        },
		mounted(){
            this.nav_dom = document.querySelector('.nav');
            console.log(this.$store.state.address_info)
            this.address_list = this.$store.state.address_info;
     
            if(this.nav_dom){
                this.nav_dom.style.display = 'none';
            }
			
		},
		methods:{
			set_default(idx){
				this.$store.commit('reset_default',idx)
			},
			edit_address(idx){
				this.$router.push({
					name:'addaddress',
					params:{index:idx}
				})
			},
			delete_address(idx){
				this.$store.commit('delete_address_info',idx)
				this.save_address_to_local();
			},
			add_address:function(){
				this.$router.push({
					name:"addaddress",//动态路由传参数要使用name配置
					params:{index:'new'}
				})
			},
			go:function(){
				this.$router.go(-1)
			},
			save_address_to_local(){
				let ls = localStorage;
				ls.setItem('address',JSON.stringify(this.$store.state.address_info))

			},
		},
        destroyed(){
            if(this.nav_dom){
                this.nav_dom.style.display = '';
            }
        }
	}
</script>
<style lang="scss" scoped>
	.bg{
		background: deepskyblue;
	}
	.wrap{
		width:100%;
		height:100%;
		display:flex;
		background:#F3F3F3;
		flex-direction:column;
		.address-header{
			height:1rem;
			background:#fff;
			flex-shrink: 0;
			border-bottom: 1px solid #D1D1D1;
			display:flex;
			-webkit-justify-content:space-between;
			line-height:1rem;
			padding-left:.15rem;
			span{
				-webkit-flex:1;
				font-size:.3rem;
				color:#7F7F7F;

			}
			.s2{
				
				text-align: center;
			}
			img{
				width:.8rem;
				height:.8rem;
				position:fixed;
				right:.15rem;
				top:.1rem;
			}
		}
		.main{
			width:100%;
			-webkit-flex:1;
			font-size: .3rem;
			img{
				width:.35rem;
				height:.35rem;
			}
			.con{
				background:#fff;
				margin-bottom: .3rem;
				.bot{
					display:flex;
					/*justify-content:center;*/
					align-items:center;
					padding:.3rem .2rem;
					color:#666;
					.p3,.p1{
						display:flex;
						align-items:center;
					}
					.p3{
						flex:1.2;
						img{
							margin-left:.2rem;
						}
						span{
							margin-left:.1rem;
						}
					}
					p{
						flex:1;
					}

				}
				.top{
					border-bottom:1px solid #D1D1D1;
					padding:.3rem .2rem;
					p{
						color:#666;
						font-size:.28rem;
						margin-top:.4rem;
						span{
							margin-left: .2rem;
						}
					}
					h4{
						font-size:.36rem;
						span{
							font-size:.3rem;
							color:#666;
							margin-left:.3rem;
							
						}
					}
				}
			}
			.circle{
				 width: .48rem;
		        height: .48rem;
		        border-radius: 50%;
		        border: 1px solid #ccc; 
		        display: inline-block;
		       	margin-right:.2rem;
			}
			.add{
				width:60%;
				height:1rem;
				font-size:.3rem;
				display:flex;
				justify-content:center;
				align-items:center;
				color:#fff;
				line-height:1rem;
				border-radius: 30px;
				background:#FB4240;
				text-align: center;
				margin:.6rem 20%;
				.s1{
					font-size:.5rem;
				}
			}
		}
	}
</style>