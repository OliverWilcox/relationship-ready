<template>
	<div class="path-container" ref="pathcont">
		<div class="path-cont-2">
			<div
				class="path-picture-container"
				ref="pathPic"
				v-bind:class="{ pathPicActive: isPathPicActive }"
			>
				<img src="../assets/pathPic.png" alt="" class="path-pic" />
			</div>
			<h1
				class="path-text"
				ref="pathTitle"
				v-bind:class="{ pathTitleActive: isPathTitleActive }"
				v-on:click="refresh"
			>
				A FRESH PATH TO DATING
			</h1>
			<p
				class="path-paragraph"
				ref="para"
				v-bind:class="{ paragraphActive: isParagraphActive }"
			>
				We can help you show up in your love life with fresh thinking,
				opening a world of possibilities. With a new way to approach
				dating - a new dating life is just one thought away.
				<br />
				<br />
				Join us to discover what gets in the way of you finding that
				real connection that you are looking for, so you can be
				Relationship Ready.
				<br />
				<br />
				The natural version of you is the upgrade.
			</p>
			<router-link to="/programmes">
				<button class="path-btn">
					<h3>LEARN ABOUT OUR PROGRAMS</h3>
				</button>
			</router-link>
			<router-link to="/work-together">
				<h3 class="path-in-touch-text">Get In Touch.</h3>
			</router-link>
		</div>
	</div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/dist/ScrollTrigger";
import { ScrollToPlugin } from "gsap/dist/ScrollToPlugin";
import { SplitText } from "gsap/dist/SplitText";
gsap.registerPlugin(ScrollToPlugin);
gsap.registerPlugin(ScrollTrigger);
gsap.registerPlugin(SplitText);
export default {
	data: () => ({
		isPathPicActive: false,
		isParagraphActive: false,

		isPathTitleActive: false,
		scrollTriggerPic: null
	}),
	mounted() {
		ScrollTrigger.create({
			trigger: ".path-container",
			toggleActions: "play none none none",
			start: () => "top " + window.innerHeight * 0.6,
			onEnter: () => (this.isParagraphActive = true)
		});

		this.scrollTriggerPic = ScrollTrigger.create({
			trigger: ".path-container",

			start: () => "top " + window.innerHeight * 0.75,
			toggleActions: "play none none none",
			onEnter: () => (this.isPathPicActive = true)
		});
		ScrollTrigger.create({
			trigger: ".path-container",
			toggleActions: "play none none none",
			onEnter: () => this.timeLine(),

			start: () => "top " + window.innerHeight * 0.75,

			onLeaveBack: (self) => self.disable()
		});
	},

	created() {
		setTimeout(function () {
			ScrollTrigger.refresh();
		}, 50);
	},
	methods: {
		timeLine() {
			this.isPathTitleActive = true;
			let tl = gsap.timeline(),
				mySplitText = new SplitText(this.$refs.pathTitle, {
					type: "lines"
				}),
				lines = mySplitText.lines;
			gsap.set(this.$refs.pathTitle, { perspective: 400 });
			tl.from(
				lines,
				{
					y: 40,
					opacity: 0,
					duration: 0.6,
					stagger: 0.1,
					rotationX: 80,
					transformOrigin: "20% 0 0"
				},
				"+=0"
			);
		},
		refresh() {
			ScrollTrigger.refresh();
		},

		prog() {}
	}
};
</script>

<style>
.path-container {
	position: relative;
	margin-top: -10vw;
	width: 100vw;
	padding-top: 8vw;

	padding-bottom: 7vw;
	overflow: hidden;
}

.path-text {
	position: relative;
	top: 1vw;
	text-align: left;
	margin-left: 11vw;
	line-height: 5.8vw;
	font-size: 6.25vw;
	width: 50vw;
	opacity: 0;
}

.pathTitleActive {
	opacity: 1;
}

.path-paragraph {
	position: relative;
	margin-top: 0.5vw;
	text-align: left;
	margin-left: 11vw;
	transition: 1s;
	font-size: 1.3vw;
	width: 50vw;
	opacity: 0;
	top: -2vw;
	font-family: DM sans;
}

.paragraphActive {
	opacity: 1;
}

.path-btn {
	position: relative;
	left: 0;
	margin-top: 2.5vw;
	width: 18vw;
	height: 4.5vw;
	display: block;
	margin-left: 11vw;
	top: -2vw;
	background: none;
	border: 0.2vw solid #d4c09e;
	cursor: pointer;
}

.path-btn h3 {
	font-size: 0.9vw;
	color: #544a3f;
	padding: 0.45vw;
}

.path-in-touch-text {
	position: relative;
	margin-top: 2.5vw;
	text-align: left;
	margin-left: 11vw;
	transition: 1s;
	font-size: 1.3vw;
	width: 50vw;
	opacity: 1;
	top: -2vw;
	font-weight: bold;
	cursor: pointer;
	font-family: DM sans;
	color: #544a3f;
	text-decoration-color: #d4c09e;
	text-decoration: underline;
}

.path-picture-container {
	position: absolute;

	width: 32vw;
	height: 40vw;
	top: 50%;
	transform: translateY(-50%);
	right: -20vw;
	overflow: hidden;
	transition: 1s;
	opacity: 0.2;
}

.path-pic {
	width: 115%;
}

.pathPicActive {
	opacity: 1;
	right: 0;
}

@media (max-aspect-ratio: 200/200) {
	.path-container {
		padding-top: 16vw;
		padding-bottom: 16vw;
	}

	.path-text {
		top: 16vw;
		line-height: 10.8vw;
		font-size: 11.25vw;
		width: 85vw;
		margin-left: 6vw;
	}

	.path-paragraph {
		margin-top: 24vw;
		font-size: 3.8vw;
		width: 75vw;
		top: -6vw;
		margin-left: 6vw;
	}

	.path-btn {
		margin-top: 10vw;
		margin-left: 6vw;
		width: 50vw;
		height: 15vw;

		border: 0.5vw solid #d4c09e;
		top: -6vw;
	}

	.path-btn h3 {
		font-size: 2.5vw;
		color: #544a3f;
		padding: 0.45vw;
	}

	.path-picture-container {
		width: 72vw;
		height: 90vw;
		top: 41%;
		opacity: 0.2;
		right: -50vw;
	}

	.pathPicActive {
		opacity: 0.6;
		right: 0;
	}

	.path-in-touch-text {
		margin-top: 5vw;

		margin-left: 6vw;

		font-size: 3.9vw;
	}
}
</style>
