<template>
	<div class="tile is-3 is-parent">
		<article class="tile is-child notification is-info">
			<p class="title is-5">Status</p>
			<div class="is-size-6" style="margin-top:5px;"><b>Bittrex pair: 'BTC-GBYTE'</b></div>
			<div class="is-size-6" style="margin-top:5px;"><b>Odex pair: 'GBYTE/{{configuration.destination_quote}}'</b></div>
			<div class="is-size-6" style="margin-top:5px;"><b>Control address:</b>
			<b-tooltip style="margin-left:5px;" type="is-dark" label="Address that is used to sign your order, don't send funds to it">
				<b-icon size="is-small" icon="help-circle-outline"></b-icon>
			</b-tooltip></div>
			<div class="is-size-7"> {{credentials.control_address || ''}}</div>
			<div v-if="!isStarting" style="margin-top:20px;">
				<b-button class="is-primary" v-if="!is_started" @click="$emit('start')">start</b-button>
				<b-button class="is-primary" v-if="is_started" @click="$emit('stop')">stop</b-button>
			</div>
			<b-loading :is-full-page="false" :active.sync="isStarting" :can-cancel="false"></b-loading>
		<div v-if="source_balances">
			<div class="is-size-6" style="margin-top:10px;"><b>Bittrex balances</b></div>
			<div class="is-size-7"><b>GBYTE</b></div>
			<div class="is-size-7" >
				<ul >
					<li>Free: {{source_balances.free.GBYTE}}</li>
					<li>Used: {{source_balances.used.GBYTE}}</li>
				</ul>
			</div>
			<div class="is-size-7"><b>BTC</b></div>
			<div class="is-size-7" >
				<ul >
					<li>Free: {{source_balances.free.BTC}}</li>
					<li>Used: {{source_balances.used.BTC}}</li>
				</ul>
			</div>
		</div>
		<div v-if="dest_balances">
			<div class="is-size-6" style="margin-top:10px;"><b>Odex balances</b></div>
			<div class="is-size-7"><b>{{pairTokens[0].symbol}}</b></div>
			<div class="is-size-7" >
				<ul >
					<li>Total: {{dest_balances[pairTokens[0].symbol]/(10 ** this.pairTokens[0].decimals)}}</li>
				</ul>
			</div>
			<div class="is-size-7"><b>{{pairTokens[1].symbol}}</b></div>
			<div class="is-size-7" >
				<ul >
					<li>Total: {{dest_balances[pairTokens[1].symbol]/(10 ** this.pairTokens[1].decimals)}}</li>
				</ul>
			</div>
		</div>


		</article>
					
	</div>
</template>

<script>
import { EventBus } from '../js/event-bus.js';

export default {
  props: {
		connections: Object,
		credentials: Object,
		configuration: Object,
		is_started: Boolean,
		pairTokens: Array,
		isStarting: Boolean
	},
	data () {
			return {
				control_address: '',
				source_balances: false,
				dest_balances: false
			}
		},
	created() {
		EventBus.$on('source_balances', (source_balances)=>{
			this.source_balances = source_balances;
		})
		EventBus.$on('dest_balances', (dest_balances)=>{
			this.dest_balances = dest_balances;
		})
	},
	watch:{
		credentials: function() {
		},
		connections: function() {
		},
		configuration: function() {
		}
		 
	},
	methods:{


	}
}
</script>

