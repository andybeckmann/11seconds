<template>
	<div id="app">
		<div class="app--clock">{{ this.clock }}</div>
		<div v-if="this.modeA.active == true" class="app--bubble modeA"></div>
		<div v-if="this.modeB.active == true" class="app--bubble modeB"></div>
		<div v-if="this.modeC.active == true" class="app--bubble modeC"></div>
		<div v-if="this.modeA.active != true && this.modeB.active != true && this.modeC.active != true" class="app--bubble"></div>
		<div class="app--controls" v-if="this.controlStatus == 'Reset'" @click="timerStop">{{ this.controlStatus }}</div>
		<div class="app--controls" v-if="this.controlStatus == 'Pick A Duration'" @click="selectTimeInterval('B')">{{ this.controlStatus }}</div>
		<div class="app--modes">
			<ul>
				<li @click="selectTimeInterval('A')" :class="{ 'active' : modeA.active }">10s</li>
				<li @click="selectTimeInterval('B')" :class="{ 'active' : modeB.active }">11s</li>
				<li @click="selectTimeInterval('C')" :class="{ 'active' : modeC.active }">12s</li>
			</ul>
		</div>
		<div v-if="this.modeA.active == true" class="app--timeline-text modeA">{{ this.breathStatus }}</div>
		<div v-if="this.modeB.active == true" class="app--timeline-text modeB">{{ this.breathStatus }}</div>
		<div v-if="this.modeC.active == true" class="app--timeline-text modeC">{{ this.breathStatus }}</div>
		<div class="app--timeline-bar">
			<div v-if="this.modeA.active == true" class="app--timeline-bar-indicator modeA"></div>
			<div v-if="this.modeB.active == true" class="app--timeline-bar-indicator modeB"></div>
			<div v-if="this.modeC.active == true" class="app--timeline-bar-indicator modeC"></div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'App',

	methods: {

		timerStart(selectedTime) {
			let midPoint = (selectedTime/2).toFixed(2)
			let i = 0.00
			let reachedEnd = false
			this.interval = setInterval(() => {
				if (i.toFixed(2) == midPoint) {
					reachedEnd = true
					this.breathStatus = 'Exhale'
				}
				if (reachedEnd == false) {
					i += 0.10
				} else {
					i -= 0.10
				}
				this.clock = i.toFixed(2)
				if (i.toFixed(2) - 0.10 == 0.00) {
					reachedEnd = false;
					this.breathStatus = 'Inhale'
				}
			}, 100)
		},

		timerStop() {
			this.modeA.active = false
			this.modeB.active = false
			this.modeC.active = false
			clearInterval(this.interval)
			this.clock = 0.00
			this.controlStatus = 'Pick A Duration'
		},

		timerReset(newCount) {
			clearInterval(this.interval)
			this.clock = 0.00
			this.timerStart(newCount)
		},

		selectTimeInterval(selection) {
			this.controlStatus = 'Reset'

			if (selection == 'A') {
				this.modeA.active = false
				this.modeB.active = false
				this.modeC.active = false
				setInterval(() => {
				}, 100)
				this.timerReset(10)
				this.modeA.active = true

			} else if (selection == 'B') {
				this.modeA.active = false
				this.modeB.active = false
				this.modeC.active = false
				setInterval(() => {
				}, 100)
				this.timerReset(11)
				this.modeB.active = true

			} else if (selection == 'C') {
				this.modeA.active = false
				this.modeB.active = false
				this.modeC.active = false
				setInterval(() => {
				}, 100)
				this.timerReset(12)
				this.modeC.active = true
			}
		}
	},

	created() {
		this.timerStart(11)
	},

	data() {
		return {
			clock: 0.00,
			controlStatus: 'Reset',
			breathStatus: 'Pick A Duration',
			modeA: {
				active: false
			},
			modeB: {
				active: true
			},
			modeC: {
				active: false
			}
		}
	}
}
</script>

<style lang="scss">
	// Fonts
	@font-face {
		font-family: 'Montserrat-Light';
		src: url('assets/fonts/Montserrat/Montserrat-Light.ttf');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}

	@font-face {
		font-family: 'Montserrat-ExtraBoldItalic';
		src: url('assets/fonts/Montserrat/Montserrat-ExtraBoldItalic.ttf');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}

	// Global
	* {
		box-sizing: border-box;
		-webkit-tap-highlight-color: rgba(0,0,0,0);
	}

	img {
		max-width: 100%;
		height: auto;
	}

	body {
		font-family: 'Montserrat-Light', sans-serif;
		padding: 0;
		margin: 0;
	}

	// App
	#app {
		background: #fff;
		height: 100vh;
		width: 100%;
		margin: 0 auto;
		position: relative;
	}

	// App: Clock
	.app--clock {
		background: #00ffaf;
		height: 80px;
		color: #074367;
		text-align: center;
		padding-top: 26px;
		font-size: 24px;
	}

	// App: Bubble
	.app--bubble {
		top: calc(50% - 200px);
		position: relative;
		background: #effbff;
		width: 125px;
		height: 125px;
		transform: scale(1);
		margin: 0 auto;
		display: flex;
		text-align: center;
		justify-content: center;
		align-items: center;
		border-radius: 50%;
		border: 25px solid #5bcaed;
		box-shadow: 0 7px 55px rgba(0,0,0,0.05);
		animation: none;

		&.modeA {
			animation: 10s bubbleBreatheA infinite;
		}

		&.modeB {
			animation: 11s bubbleBreatheB infinite;
		}

		&.modeC {
			animation: 12s bubbleBreatheC infinite;
		}
	}

	@keyframes bubbleBreatheA {
		0% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}

		50% {
			transform: scale(2);
			border: 0px solid transparent;
			background: #00ffaf;
		}

		100% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}
	}

	@keyframes bubbleBreatheB {
		0% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}

		50% {
			transform: scale(2);
			border: 0px solid transparent;
			background: #00ffaf;
		}

		100% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}
	}

	@keyframes bubbleBreatheC {
		0% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}

		50% {
			transform: scale(2);
			border: 0px solid transparent;
			background: #00ffaf;
		}

		100% {
			transform: scale(1);
			border: 25px solid #116394;
			background: #fff;
		}
	}

	// App: Controls
	.app--controls {
		color: #074367;
		text-align: center;
		font-size: 20px;
		text-transform: uppercase;
		position: absolute;
		width: 100%;
		margin: 25px 0;
		bottom: 185px;
		left: 0;
	}

	// App: Modes
	.app--modes {
		position: absolute;
		margin: 25px;
		padding: 20px;
		bottom: 55px;
		left: 0;
		width: calc(100% - 50px);
		background: #effbff;
		border-radius: 25px;
	
		ul {
			margin: 0;
			padding: 0;
			list-style-type: none;
			display: flex;
			justify-content: space-between;

			li {
				display: inline;
				font-size: 30px;
				line-height: 10px;
				padding: 25px 15px;
				margin: 0 5px;
				color: #116394;
				cursor: pointer;

				&.active {
					background: #116394;
					border-radius: 5px;
					font-family: 'Montserrat-ExtraBoldItalic', sans-serif;
					color: #fff;
					border-radius: 20px;
				}
			}
		}
	}

	// App: Timeline
	.app--timeline-text {
		position: absolute;
		bottom: 25px;
		left: 15px;
		content: 'in';
		text-transform: uppercase;
		color: #116394;
		font-size: 28px;

		&.modeA {
			animation: 10s textBreathA infinite;
		}

		&.modeB {
			animation: 11s textBreathB infinite;
		}

		&.modeC {
			animation: 12s textBreathC infinite;
		}
	}

	@keyframes textBreathA {
		0% {
			content: 'Inhale';
		}

		49.999% {
			content: 'Inhale'; 
		}

		50% {
			content: 'Exhale';
		}

		100% {
			content: 'Exhale';
		}
	}

	@keyframes textBreathB {
		0% {
			content: 'Inhale';
		}

		49.999% {
			content: 'Inhale'; 
		}

		50% {
			content: 'Exhale';
		}

		100% {
			content: 'Exhale';
		}
	}

	@keyframes textBreathC {
		0% {
			content: 'Inhale';
		}

		49.999% {
			content: 'Inhale'; 
		}

		50% {
			content: 'Exhale';
		}

		100% {
			content: 'Exhale';
		}
	}

	.app--timeline-bar {
		width: 100%;
		height: 15px;
		background: #116394;
		position: absolute;
		bottom: 0;
		left: 0;

		.app--timeline-bar-indicator {
			background: #00ffaf;
			height: 15px;
			position: absolute;
			bottom: 0;
			left: 0;
			width: 0%;

			&.modeA {
				animation: 10s timelineAnimateA infinite;
			}

			&.modeB {
				animation: 11s timelineAnimateB infinite;
			}

			&.modeC {
				animation: 12s timelineAnimateC infinite;
			}
		}
	}

	@keyframes timelineAnimateA {
		0% {
			width: 0%;
		}

		50% {
			width: 100%;
		}

		100% {
			width: 0%;
		}
	}

	@keyframes timelineAnimateB {
		0% {
			width: 0%;
		}

		50% {
			width: 100%;
		}

		100% {
			width: 0%;
		}
	}

	@keyframes timelineAnimateC {
		0% {
			width: 0%;
		}

		50% {
			width: 100%;
		}

		100% {
			width: 0%;
		}
	}
</style>
