<template>
	<div class="game">
		<div id="head">
			<v-row>
				<v-col cols="2" offset=1>
					<v-row no-gutters>
						<v-col cols="3">
							<v-btn @click="back"  color="rgb(50,183,213)">
								退出
							</v-btn>
						</v-col>
						<v-col cols="4" offset=1>
							<v-btn>
								声音
							</v-btn>
						</v-col>
					</v-row>
				</v-col>
				
				<v-col  cols="8">
					<Money></Money>
				</v-col>
			</v-row>
		</div>
		<div id="body">
			<!-- 对家 -->
			<div class="gameOpposite">
				<div class="oppositeImg">
					<img :src="oppositePlayer.uface" width="70" height="70" />
					&nbsp;{{oppositePlayer.uname}}
				</div>
				<div>
					
					<v-card class="oppositeState" v-if="oppositePlayer.status == 0" color="#CC0033" width="220" height="50" outlined>
						<v-card-text>
							<span style="font-size: 18px;color: white;">
								未准备
							</span>
						</v-card-text>
					</v-card>
					<v-card class="oppositeState" v-if="oppositePlayer.status == 1" color="#556B2F" width="220" height="50" outlined>
						<v-card-text>
							<span style="font-size: 18px;color: white;">
								已准备
							</span>
						</v-card-text>
					</v-card>
					<v-card class="oppositeState" v-if="oppositePlayer.status == 2" color="#008000" width="220" height="50" @click="begin">
						<v-card-text>
							<span style="font-size: 18px;color: white;">
								等待开始游戏
							</span>
						</v-card-text>
					</v-card>
					<v-card class="oppositeState" v-if="oppositePlayer.status == 4" color="#808080" width="220" height="50" @click="begin">
						<v-card-text>
							<span style="font-size: 18px;color: white;">
								等待玩家加入
							</span>
						</v-card-text>
					</v-card>
					
				</div>
			</div>
			<!-- 临家 -->
			<div class="gameLimb">
				<div class="leftPlayer">
					<img :src="leftPlayer.uface" width="70" height="70" style="margin-top: 3px;" /><br>
					&nbsp;{{leftPlayer.uname}}
				</div>
				
				<v-card class="leftPlayerState" v-if="leftPlayer.status == 0"C color="#CC0033" width="100" height="100" outlined>
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							<br/>
							未准备
						</span>
					</v-card-text>
				</v-card>
				<v-card class="leftPlayerState" v-if="leftPlayer.status == 1" color="#556B2F" width="100" height="100" outlined>
					<v-card-text>
						<span style="font-size: 18px;margin-top: 10px; color: white">
							<br/>
							已准备
						</span>
					</v-card-text>
				</v-card>
				<v-card class="leftPlayerState" v-if="leftPlayer.status == 2" color="#008000" width="100" height="100" @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							等待<br/>开始<br/>游戏
						</span>
					</v-card-text>
				</v-card>
				<v-card class="leftPlayerState" v-if="leftPlayer.status == 4" color="#808080" width="100" height="100" @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							等待<br/>玩家<br/>加入
						</span>
					</v-card-text>
				</v-card>
				
				
				<div class="rightPlayer">
					<img :src="rightPlayer.uface" width="70" height="70" style="margin-top: 3px;" /><br>
					&nbsp;{{rightPlayer.uname}}
				</div>

				<v-card class="rightPlayerState" v-if="rightPlayer.status == 0" color="#CC0033" width="100" height="100" outlined @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							<br />
							未准备
						</span>
					</v-card-text>
				</v-card>
				<v-card class="rightPlayerState" v-if="rightPlayer.status == 1" color="#556B2F" width="100" height="100" outlined @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							<br>
							已准备
						</span>
					</v-card-text>
				</v-card>
				<v-card class="rightPlayerState" v-if="rightPlayer.status == 2" color="#008000" width="100" height="100" @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							等待<br/>开始<br/>游戏
						</span>
					</v-card-text>
				</v-card>
				<v-card class="rightPlayerState" v-if="rightPlayer.status == 4" color="#808080" width="100" height="100">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							等待<br/>玩家<br/>加入
						</span>
					</v-card-text>
				</v-card>
				
				
				
			</div>
			<!-- 自己 -->
			<div class="gameMySelf">
				<div>
					<v-bottom-sheet v-model="inviteFriends" inset><!-- 此处留有问题，由于上边使用了v-for,应该解决  已解决-->
						<template v-slot:activator="{ on }">
							<v-btn class="myselfInvite" large color="primary" width=120 v-on="on">
								邀请好友
							</v-btn>
						</template>
						<v-sheet style="overflow-y:auto;" class="text-center" height="300px" width="300px">
							<v-btn class="mt-6" text color="error" @click="inviteFriends = !inviteFriends">关闭</v-btn>
							<v-list>
								<v-list-item v-for="(friend, index) in friends" :key="index">
									<v-list-item-title>
										<v-row>
											<v-col cols="7" offset="1">
												<div class="my-3">{{ friend.uname }}</div>
											</v-col>
											<v-col cols="3">
												<v-btn color="warning" fab small>
													<v-icon>mdi-account-circle</v-icon>
												</v-btn>
											</v-col>
										</v-row>
									</v-list-item-title>
								</v-list-item>
							</v-list>
						</v-sheet>
					</v-bottom-sheet>
				</div>



				<v-card class="mySelfState" v-if="mySelfPlayer.status == 2" color="#008000" width="220" height="50" @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							开始游戏
						</span>
					</v-card-text>
				</v-card>
				<v-card class="mySelfState" v-if="mySelfPlayer.status == 0" color="#CC0033" width="220" height="50" outlined @click="changeStatus(0)">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							未准备
						</span>
					</v-card-text>
				</v-card>
				<!-- <v-card class="mySelfState" v-if="mySelfPlayer.status == 1" color="#556B2F" width="220" height="50" outlined  @click="changeStatus(1)"> -->
				<v-card class="mySelfState" v-if="mySelfPlayer.status == 1" color="#556B2F" width="220" height="50" outlined  @click="begin">
					<v-card-text>
						<span style="font-size: 18px;color: white;">
							已准备
						</span>
					</v-card-text>
				</v-card>
				
				
				
				<div class="mySelfImg">
					<img :src="mySelfPlayer.uface" style="margin: 10px;" width="70" height="70" />
					<span class="myName">&nbsp;{{mySelfPlayer.uname}}</span>
				</div>
				
				<div>
					
					<v-bottom-sheet v-model="chatFriends" inset hide-overlay>
						<template v-slot:activator="{ on }">
							<v-btn class="myselfChat" large color="primary" width=120 v-on="on">
								聊天
							</v-btn>
						</template>
						<v-sheet style="overflow-y:auto; float: right;" class="text-center" height="300px" width="300px">
							<v-btn class="mt-6" text color="error" @click="chatFriends = !chatFriends">关闭</v-btn>
							<v-list>
								<v-list-item v-for="(friend, index) in friends" :key="index">
									<v-list-item-title>
										<v-row>
											<v-col cols="7" offset="1">
												<div class="my-3">{{ friend.uname }}</div>
											</v-col>
											<v-col cols="3">
												<v-btn color="warning" fab small @click="sendToFriend()">
													<v-icon>mdi-message-text</v-icon>
												</v-btn>
											</v-col>
										</v-row>
									</v-list-item-title>
								</v-list-item>
							</v-list>
						</v-sheet>
					</v-bottom-sheet>
					<!-- <v-text-field label="testewstewgioadejfiodajfoif" v-model="test"></v-text-field> -->
					
					<!-- <v-btn class="myselfChat" large color="primary" width=120>
						聊天
					</v-btn> -->
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import {mapState} from 'vuex'
	// 金币钻石
	import Money from '../../components/head/money.vue'
	export default {		
		components:{
			Money,
		},
		data() {
			return {
				user : JSON.parse(window.sessionStorage.getItem("user")),//当前用户
				isInit:true,
				inviteFriends: false,
				chatFriends:false,
			}
		},
        computed: mapState([
            'stomp_room',
			'roomId',
			'index',
			'friends',
			'mySelfPlayer',
			'oppositePlayer',
			'leftPlayer',
			'rightPlayer',
			'test',
        ]),
		methods: {
			back() {
				// 发送推出消息
				let msgObj = new Object();//发送加入房间信息
				msgObj.type = 1;//0 加入房间 1 退出房间
				msgObj.roomId = this.roomId;
				let from = new Object();
				from.username= this.user.username;
				msgObj.from = from;
				this.stomp_room.send('/ws/sendAllUser/'+this.roomId, {}, JSON.stringify(msgObj));
				//关闭房间socket连接
				// this.$store.dispatch('close_connect_room');
				this.$router.push("/choseRoom");
			},
			begin() {
				//开始游戏之前判断所有玩家是否已准备
				if(this.leftPlayer.status == 1 && this.rightPlayer.status == 1 && this.oppositePlayer.status == 1){
					//若全部准备，开始游戏进入 游戏即可
					let msgObj = new Object();
					msgObj.type = 5;
					this.stomp_room.send('/ws/sendAllUser/'+this.roomId, {}, JSON.stringify(msgObj));
					
				}else{
					alert("还有玩家未准备呢！");
				}
				
				
			},
			sendToFriend(){
				alert(this.count);
			},
			sendMessage(){
				if(this.isInit){
					let msgObj = new Object();//发送加入房间信息
					msgObj.type = 0;//0 加入房间 1 退出房间
					msgObj.roomId = this.roomId;
					let from = new Object();
					from.username= this.user.username;
					msgObj.from = from;
					this.stomp_room.send('/ws/sendAllUser/'+this.roomId, {}, JSON.stringify(msgObj));
					this.isInit = false;
				}else{
					alert(22323);
				}
			},
			/**
			 * @param {Object} curType 改变自己的状态 0、从未准备切换到已准备状态 1、与之相反
			 */
			changeStatus(curType){
				
				// this.getRequest("/cendTo", this.user).then(s => {});
				/**
				 * 参数说明：
				 * msgObj.type: 0、加入房间 1、退出房间 2、从未准备状态切换到准备状态，3、从准备状态切换到未准备状态
				 * msgObj.index: 当前玩家所在数组下标
				 * this.roomId:房间id
				 */
				if(curType == 0){// 切换到已准备状态
					let msgObj = new Object();
					msgObj.type = 2;
					msgObj.index = this.index; 
					this.stomp_room.send('/ws/sendAllUser/'+this.roomId, {}, JSON.stringify(msgObj));
				// alert(1);
				}else if(curType == 1){// 切换到未准备状态
					let msgObj = new Object();
					msgObj.type = 3;
					msgObj.index = this.index;
					this.stomp_room.send('/ws/sendAllUser/'+this.roomId, {}, JSON.stringify(msgObj));
					
				}
			}
			// perpare(){
			// 	this.$router.push("/game");
			// }
		},
		created() {
			this.sendMessage();
		},
		watch:{
			players: {
				handler: function() {
				}, 
				// deep: true
			},
		}
	}
</script>

<style>
	.game {
		width: 100%;
		height: 100%;
	}

	#head {
		float: left;
		background-color: #ACE0F9;
		width: 100%;
		height: 75px;
	}

	#body {
		width: 100%;
		height: 100%;
		background-color: #B0C4DE;
	}

	.gameOpposite {
		height: 30%;
		/* background-color: #67C23A; */
		text-align: center;
	}

	.gameLimb {
		height: 45%;
		/* background-color: #B0C4DE; */
	}

	.leftPlayer {
		position: absolute;
		padding-top: 15px;
		padding-left: 20px;
		height: 100px;
		width: 100px;
		left: 10px;
		top: 45%;
		background-color: #ACE0F9;
	}

	.rightPlayer {
		text-align: right;
		position: absolute;
		padding-top: 15px;
		padding-right: 20px;
		height: 100px;
		width: 100px;
		right: 10px;
		top: 45%;
		background-color: #ACE0F9;
	}

	.gameMySelf {
		height: 25%;
		width: 100%;
		text-align: center;
		/* background-color: #FFCCFF; */
	}

	.oppositeImg {
		margin-left: 40%;
		width: 220px;
		height: 130px;
		padding-top: 80px;
		/* background-color: #B0C4DE; */
	}

	.myselfInvite {
		/* 		position: absolute;
		bottom: 10px;
		margin-left: 10%; */
		position: absolute;
		left: 0px;

	}

	.myselfChat {
		/* 	position: absolute;
		bottom: 10px;
		margin-left: 56%; */
		position: absolute;
		left: 0px;
	}

	.mySelfImg {
		position: absolute;
		bottom: 5px;
		margin-left: 40%;
		width: 220px;
		height: 60px;
		background-color: aqua;
	}

	.myName {
		position: absolute;
		bottom: 15px;
		left: 70px;
	}

	/* 准备装状态 */
	/* 对置为已准备状态 */
	.prepare {
		background-color: burlywood;
	}

	.noPrepare {
		background-color: whitesmoke;
	}

	.begin {
		background-color: #67C23A;
	}

	.oppositeState {
		margin-left: 40%;
		margin-top: 10px;
	}

	.leftPlayerState {
		position: absolute;
		left: 135px;
		top: 45%;
		/* padding-top: 10px; */
	}

	.rightPlayerState {
		position: absolute;
		right: 135px;
		top: 45%;
	}

	.blacky {
		background-color: #000000;
	}

	.mySelfState {
		position: absolute;
		left: 40%;
		bottom: 75px;
		background-color: #000000;
	}
</style>
