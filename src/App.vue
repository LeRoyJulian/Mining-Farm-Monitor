<template>
	<div id="app">
		<div class="container-fluid">
			<h1>Mining Farm Monitor</h1>
			<h2><b>{{ json.hashRate }}</b></h2>
			<div class="row indicators">
				<div class="col-sm-4">
					<div class="panel panel-default">
						<div class="panel-body">
							{{ this.json.totalShareStats.valid }}
						</div>
						<div class="panel-footer">
							Valid Shares
						</div>
					</div>
				</div>
				<div class="col-sm-4">
					<div class="panel panel-default">
						<div class="panel-body">
							{{ (this.json.ethPerMin * 60).toFixed(6) }}
						</div>
						<div class="panel-footer">
							ETH/h
						</div>
					</div>
				</div>
				<div class="col-sm-4">
					<div class="panel panel-default">
						<div class="panel-body">
							{{ (this.json.ethPerMin * 60 * 24).toFixed(6) }}
						</div>
						<div class="panel-footer">
							ETH/d
						</div>
					</div>
				</div>
			</div>
			<div class="row indicators">
				<div class="col-sm-6">
					<div class="panel panel-default">
						<div class="panel-body">
							{{ this.json.credits[0].credit / 1000000000000 }}<sup>T</sup>
						</div>
						<div class="panel-footer">
							Credits
						</div>
					</div>
				</div>
				<div class="col-sm-6">
					<div class="panel panel-default">
						<div class="panel-body">
							{{ this.daysBeforeReward }}
						</div>
						<div class="panel-footer">
							Days Before Next Reward
						</div>
					</div>
				</div>
			</div>
			<div class="panel panel-default">
				<div class="panel-body workers">
					<div class="worker" v-for="worker in json.workers">
						<i class="fa fa-3x fa-server"></i><div><b><i class="fa fa-circle" :class="isWorkerActive(worker.workerLastSubmitTime)"></i> {{ worker.worker }}</b><br /><small>{{ dateFormated(worker.workerLastSubmitTime) }}</small></div>
					</div>
				</div>
			</div>
			<div class="panel panel-default">
				<div class="panel-body">
					<p>a8cCce14A61453a317F3D867a1c4c11CF75EFBD7</p>
				</div>
				<div class="panel-footer">
					<a :href="walletURL"><i class="fa fa-credit-card"></i></a>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  name: 'app',
	data: function () {
		return {
			'lastAlert': 0,
			'json': {
				'address': '',
				'ethPerMin': 0,
				'workers': {},
				'totalShareStats': {
					'valid': 0
				},
				'credits': [
					{
						'credit' : 0
					}
				]
			}
		}
	},
	computed: {
		walletURL: function () {
			return 'https://etherchain.org/account/0x' + this.json.address
		},
		daysBeforeReward: function() {
			return ((this.json.credits[0].maxCredit - this.json.credits[0].credit) / (this.json.totalShareStats.valid * 4000000000000) * 24).toFixed(2)
		}
	},
	created() {
		this.getJSON();
  },
  methods: {
	  getJSON() {
			console.log('Fetching...');
			var self = this;
			axios.get('/ethpool.php')
				.then(function (response) {
					console.log('Fetched!');
					self.json = response.data;
               console.log(self.json);
					setTimeout(function(){ self.getJSON() }, 5000);
				})
				.catch(function (error) {
					console.log(error);
				});
		},
		isWorkerActive(timestamp) {
			if (this.lastAlert < moment().subtract(1, 'minutes').unix()) {
				if (timestamp < moment().subtract(15, 'minutes').unix()) {
					this.alertWorkerOffline();
				}
			}
			return {
				'text-success' : timestamp >= moment().subtract(15, 'minutes').unix(),
				'text-danger' : timestamp < moment().subtract(15, 'minutes').unix(),
			}
		},
		alertWorkerOffline() {
         var audio = new Audio('./static/210-game-over.mp3');
         audio.play();
         this.lastAlert = moment().unix();
		},
		dateFormated(timestamp) {
			moment.locale('fr');
			return moment.unix(timestamp).fromNow() ;
		}
  }
}
</script>

<style>

body {padding: 0; margin: 0; }

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 45px 0px;
  background: linear-gradient(to bottom, rgba(255,255,255,0.15) 0%, rgba(0,0,0,0.15) 100%), radial-gradient(at top center, rgba(255,255,255,0.40) 0%, rgba(0,0,0,0.40) 120%) #989898;
  background-blend-mode: multiply,multiply;
  height: 100vh;
  overflow: hidden;
  margin: 0;
}

.container-fluid { max-width: 640px; }

h1 { margin-top: 0; }
h2 { margin-bottom: 45px; }
h1, h2 { font-weight: normal; }
ul { list-style-type: none; padding: 0; }
ul li { display: inline-block; margin: 0 10px; }
a { color: #42b983; }

.panel {  margin: 0 auto; margin-bottom: 30px; }
.indicators .panel-body { font-size: 22px; font-weight: bold; }
.container-fluid > .panel:last-of-type { margin-bottom: 0; }
.panel-body p:last-of-type { margin-bottom: 0; }

.workers { display: flex; align-items: center; justify-content: space-around; text-align: left; }
.workers .worker > div { float: left; }
.workers .fa-server { vertical-align: middle; margin-right: 15px; float: left; }
.workers small { color: #3f3f3f; text-transform: uppercase; font-size: 11px; }

@media (max-width: 767px) {
	#app { height: auto; }
	.panel { margin-bottom: 15px; }
}

</style>
