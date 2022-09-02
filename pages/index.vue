<template>
	<div id="app" class="theme-blue">
			<!-- Header -->
		<!-- <div class="row"> -->
			<div class="col">
				
			<div class="row row-cols">
           <div class="col " id="nameteam"><b style="font-size:100px ">Agrictakuse</b></div>
            <div class="col"><br><Logoku /><br></br>
				
			</div>
		</div>
				<!-- Content -->
		<Content v-if="isPage == 0" :payload-message="payloadMessageSensor" />
		<PreviewImage v-if="isPage == 1" :item="payloadMessageImage" />


		<div id="BT_Link" class="link_camera">
			<NuxtLink to="camera/"> <a>เรียกดูข้อมูล</a></NuxtLink>
		</div>	
	</div>
	</div>
</template>


<script>
import mqtt from 'mqtt';
import { v4 as uuidv4 } from 'uuid';

export default {
	data() {
		return {
			isPage: 0,
			payloadMessageSensor: '',
			payloadMessageImage: {
				"status": "end",
				"originalImage": "",
				"ndviImage": "",
			},
			connection: {
				// host: 'broker.emqx.io',
				host: '192.168.1.100',
				port: 8083,
				endpoint: '/mqtt',
				clean: true, // Reserved session
				connectTimeout: 4000, // Time out
				reconnectPeriod: 4000, // Reconnection interval
				// Certification Information
				clientId: `nuxtjs_farm_${uuidv4()}`,
				username: '',
				password: '',
			},
			subscription: [
				{
					topic: 'plant/dashboard/value',
					qos: 0,
				},
				{
					topic: 'plant/dashboard/image',
					qos: 0,
				},
			],
			receiveNews: '',
			client: {
				connected: false,
			},
			subscribeSuccess: false,
		};
	},
	methods: {
		createConnection() {
			// Connect string, and specify the connection method used through protocol
			// ws unencrypted WebSocket connection
			// wss encrypted WebSocket connection
			// mqtt unencrypted TCP connection
			// mqtts encrypted TCP connection
			// wxs WeChat mini app connection
			// alis Alipay mini app connection
			const { host, port, endpoint, ...options } = this.connection
			const connectUrl = `ws://${host}:${port}${endpoint}`
			try {
				this.client = mqtt.connect(connectUrl, options)
			} catch (error) {
				console.log('mqtt.connect error', error)
			}
			this.client.on('connect', () => {
				console.log('Connection succeeded!')
				this.doSubscribe()
			})
			this.client.on('error', error => {
				console.log('Connection failed', error)
			})
			this.client.on('message', (topic, message) => {
				this.receiveNews = this.receiveNews.concat(message)
				// console.log(`Received message ${message} from topic ${topic}`)

				if (topic == "plant/dashboard/value") {
					this.payloadMessageSensor = message.toString();
				}
				else if (topic == "plant/dashboard/image") {
					const jsonString = Buffer.from(message)
					const parseData = JSON.parse(jsonString)
					this.payloadMessageImage = parseData

					this.isPage = 1

					if (parseData.status == 'end') {
						setTimeout(() => {
							this.isPage = 0
						}, 10000)
					}
				}
			})
		},
		doSubscribe() {
			var qos = 0;
			this.subscription.forEach((sub) => {
				this.client.subscribe(sub.topic, { qos }, (error, res) => {
					if (error) {
						console.log('Subscribe to topics error', error)
						return
					}
					this.subscribeSuccess = true
					console.log('Subscribe to topics res', res)
				})
			});
		},
	},
	created() {
		// init mqtt
		this.createConnection()
	},
	beforeDestroy() { },

};
</script>


<style>
.header {
	padding-top: 10px;
}


.col {
	color: rgb(0, 0, 0);

}

.link_camera {
	font-size: 50px;
	text-align: center;
	border-style: solid;
}

#app.theme-blue {
	background-color: rgb(77, 77, 77);
}

#data.Content{
	background-color: #2a374a;
}
#BT_Link.link_camera {
	background-color: #fff;
	margin-bottom: 6rem;
	text-align: end;
	width: 50rem;
	margin-left: 23rem;
	border-radius: 30px;

}
.topic-head{
	text-align:center;
	color: #fff;
}
a{
 text-decoration:none; 
 color: rgb(5, 5, 5);
}
#id{
	margin-top: 10rem;
}
#nameteam{
	padding-left: 150px;
	color: #fff;
	
}
</style>
