<template>
	<div
		class="jessica-experience"
		v-for="personTwo in personTwoExperiences"
		:key="personTwo._id"
	>
		<div class="jessica-picture-container">
			<img
				class="jessica-img"
				v-if="personTwo.personTwoImage"
				:src="imageUrlFor(personTwo.personTwoImage)"
			/>
		</div>
		<h1 class="jessica-name" ref="title">{{ personTwo.personTwoTitle }}</h1>

		<p class="experience-1" ref="expOne">
			{{ personTwo.personTwoParagraphTextOne }}
		</p>
		<div class="video">
			<iframe
				width="100%"
				height="100%"
				:src="personTwo.vimeoUrl"
				frameborder="0"
				allow="autoplay; encrypted-media"
				allowfullscreen=""
			></iframe>
		</div>

		<p class="experience-2" ref="expTwo">
			{{ personTwo.personTwoParagraphTextTwo }}
			<br />
			<br />
			{{ personTwo.personTwoParagraphTextThree }}
		</p>
		<div class="other-experiences-container">
			<div class="experience-2-container">
				<h1 class="exp-name">{{ personTwo.personOneName }}</h1>
				<router-link to="/experience-one">
					<button class="person-two-button person-button">
						<h3>HEAR {{ personTwo.personOneName }}'S STORY</h3>
					</button>
				</router-link>
				<div class="experience-2-img-container">
					<img
						v-if="personTwo.personOneImage"
						:src="imageUrlFor(personTwo.personOneImage)"
						alt=""
						class="experience-2-img"
					/>
				</div>
			</div>
			<div class="experience-3-container">
				<h1 class="exp-name">{{ personTwo.personThreeName }}</h1>
				<router-link to="/experience-three">
					<button class="person-three-button person-button">
						<h3>HEAR {{ personTwo.personThreeName }}'S STORY</h3>
					</button>
				</router-link>
				<div class="experience-3-img-container">
					<img
						v-if="personTwo.personThreeImage"
						:src="imageUrlFor(personTwo.personThreeImage)"
						alt=""
						class="experience-3-img"
					/>
				</div>
			</div>
		</div>
		<WorkTogether />
	</div>
</template>

<script>
import sanity from "../client";
import imageUrlBuilder from "@sanity/image-url";
import { gsap } from "gsap/dist/gsap";
import { ScrollTrigger } from "gsap/dist/ScrollTrigger";
import { ScrollToPlugin } from "gsap/dist/ScrollToPlugin";
import { SplitText } from "gsap/dist/SplitText";
gsap.registerPlugin(ScrollToPlugin);
gsap.registerPlugin(ScrollTrigger);
gsap.registerPlugin(SplitText);
const imageBuilder = imageUrlBuilder(sanity);
const queryPersonTwo = `*[_type == "personTwo"]{
  _id,
  personTwoTitle,
  personTwoImage,
  personTwoTitle,
  personTwoParagraphTextOne,
  personTwoParagraphTextTwo,
  personTwoParagraphTextThree,
  personOneName,
  personOneImage,
  personThreeName,
  personThreeImage,
  vimeoUrl


}[0...100]`;

import WorkTogether from "../components/WorkTogether.vue";
export default {
	components: {
		WorkTogether
	},
	data: () => ({
		personTwoExperiences: []
	}),
	updated() {
		this.timeLine();
		ScrollTrigger.create({
			trigger: ".experience-1",
			toggleActions: "play none none none",
			onEnter: () => this.timeLineExpOne(),

			onLeaveBack: (self) => self.disable()
		});
		ScrollTrigger.create({
			trigger: ".experience-2",
			toggleActions: "play none none none",
			onEnter: () => this.timeLineExpTwo(),

			onLeaveBack: (self) => self.disable()
		});
	},
	methods: {
		fetchDataPersonTwo() {
			this.error = this.personTwo = null;
			this.loading = true;
			sanity.fetch(queryPersonTwo).then(
				(personTwoExperiences) => {
					this.loading = false;
					this.personTwoExperiences = personTwoExperiences;
				},
				(error) => {
					this.error = error;
				}
			);
		},
		imageUrlFor(source) {
			return imageBuilder.image(source);
		},
		timeLine() {
			let tl = gsap.timeline(),
				mySplitText = new SplitText(this.$refs.title, {
					type: "lines"
				}),
				lines = mySplitText.lines;
			gsap.set(this.$refs.lilaTitle, { perspective: 400 });
			tl.from(
				lines,
				{
					y: 0,
					opacity: 0,
					duration: 0.5,
					transformOrigin: "20% 0 0"
				},
				"+=0"
			);
		},
		timeLineExpOne() {
			this.$refs.expOne.style.opacity = 1;
		},
		timeLineExpTwo() {
			this.$refs.expTwo.style.opacity = 1;
		}
	},
	created() {
		this.fetchDataPersonTwo();
		setTimeout(function () {
			ScrollTrigger.refresh();
		}, 50);
	}
};
</script>

<style scoped>
.work-together-container {
	position: relative;
	background: none;
	top: 12.5vw;
}

h1 {
	text-transform: uppercase;
}

.video {
}

@media (max-aspect-ratio: 200/200) {
	.work-together-container {
		margin-top: 20vw;
	}
}
</style>
