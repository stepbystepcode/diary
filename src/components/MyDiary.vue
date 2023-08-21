<template>
	<div class="diary light">
		<div class="color-bg-list q-layout q-layout--standard">
			<header
				class="q-header q-layout__section--marginal bg-normal text-primary shadow-transition color-bg-content fixed-top"
			>
				<div class="header row items-center justify-between q-px-md">
					<button
						tabindex="0"
						type="button"
						onclick="window.location.replace('https://www.stepbystep.cf/')"
						role="button"
						class="q-btn q-btn-item non-selectable no-outline q-btn--flat q-btn--round text-primary q-btn--actionable q-focusable q-hoverable q-btn--wrap"
					>
						<span class="q-focus-helper"></span
						><span class="q-btn__wrapper col row q-anchor--skip"
							><span
								class="q-btn__content text-center col items-center q-anchor--skip justify-center row"
								><i aria-hidden="true" role="img" class="material-icons q-icon notranslate"
									><svg
										xmlns="http://www.w3.org/2000/svg"
										class="icon icon-tabler icon-tabler-home"
										width="24"
										height="24"
										viewBox="0 0 24 24"
										stroke-width="2"
										stroke="#52a2c4"
										fill="none"
										stroke-linecap="round"
										stroke-linejoin="round"
									>
										<desc>Download more icon variants from https://tabler-icons.io/i/home</desc>
										<path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
										<polyline points="5 12 3 12 12 3 21 12 19 12"></polyline>
										<path d="M5 12v7a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-7"></path>
										<path d="M9 21v-6a2 2 0 0 1 2 -2h2a2 2 0 0 1 2 2v6"></path></svg></i></span
						></span>
					</button>
					<div class="text-header color-text">Step by step の Diary</div>
					<button
						tabindex="0"
						type="button"
						role="button"
						@click="changeTheme"
						class="q-btn q-btn-item non-selectable no-outline q-btn--flat q-btn--round text-primary q-btn--actionable q-focusable q-hoverable q-btn--wrap"
					>
						<span class="q-focus-helper"></span
						><span class="q-btn__wrapper col row q-anchor--skip"
							><span
								class="q-btn__content text-center col items-center q-anchor--skip justify-center row"
								><i
									aria-hidden="true"
									role="img"
									class="material-icons q-icon notranslate theme-toggle"
									><svg
										xmlns="http://www.w3.org/2000/svg"
										class="icon icon-tabler icon-tabler-moon"
										width="24"
										height="24"
										viewBox="0 0 24 24"
										stroke-width="2"
										stroke="#52a2c4"
										fill="none"
										stroke-linecap="round"
										stroke-linejoin="round"
									>
										<desc>Download more icon variants from https://tabler-icons.io/i/moon</desc>
										<path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
										<path
											d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z"
										></path></svg></i
								><!----></span
							></span
						>
					</button>
				</div>
			</header>
			<div class="content list-content">
				<div v-for="(article, index) in lists" :key="index">
					<div class="bg-content list-card color-bg-content">
						<div class="web-font q-card__section q-card__section--vert">
							<div class="row items-baseline">
								<div class="list-day-text color-text">
									{{ moments(index, "D") }}
								</div>
								<div class="list-time-text q-ml-md col-grow color-desc">
									{{ moments(index, "M") }}月 · {{ moments(index, "T") }}
								</div>
								<div class="list-category-text row items-center color-desc" style="display: none">
									<i
										aria-hidden="true"
										role="presentation"
										class="q-mr-sm material-icons q-icon notranslate"
										style="font-size: 18px"
										>bookmark</i
									>
								</div>
							</div>
							<div
								class="list-content-text q-mt-md color-text"
								style="word-break: break-all; overflow-wrap: break-word"
								v-html="article.content"
							></div>
							<div class="row items-center q-mt-md justify-start">
								<div v-if="imgList[index] != ''">
									<!-- <img class="list-image q-mr-sm" v-for="(img, i) in imgList[index]" :key="i" :src="img" :alt="img"> -->
									<lightgallery
										:settings="{ speed: 500, plugins: plugins }"
										:onInit="onInit"
										:onBeforeSlide="onBeforeSlide"
									>
										<a v-for="(img, i) in imgList[index]" :key="i" :href="`https://img.gejiba.com/images/${img}`">
											<img class="list-image q-mr-sm" alt="img1" :src="`https://img.gejiba.com/images/${img}`" />
										</a>
									</lightgallery>
								</div>
							</div>
							<div class="row items-center q-mt-md no-wrap">
								<div class="list-desc-text color-desc">
									{{ article.content.length - 7 }} 字 · Redmi 10X · {{ article.weather }}
								</div>
								<div class="q-space"></div>
							</div>

							<!-- <div class="time">{{ monents(index, "T") }}</div> -->
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import Lightgallery from "lightgallery/vue";
import lgThumbnail from "lightgallery/plugins/thumbnail";
import lgZoom from "lightgallery/plugins/zoom";
export default {
	components: {
		Lightgallery,
	},
	data() {
		return {
			lists: [],
			imgList: [],
			plugins: [lgThumbnail, lgZoom],
		};
	},
	mounted() {
		this.axios.get("https://hello.stepbystep.cf/fetch.php").then((res) => {
			//this.lists = res.data.reverse();
			this.lists = res.data;
			console.log(this.lists);
			for (let i = 0; i < this.lists.length; i++) {
				let array = this.lists[i].img.split(" ");
				this.imgList.push(array);
				console.log(this.imgList);
			}
		});
	},
	methods: {
		moments(index, type) {
			let time = new Date(this.lists[index].date * 1000);
			switch (type) {
				case "Y":
					return time.getFullYear();
				case "M":
					return time.getMonth();
				case "D":
					return time.getDate();
				case "T":
					return `${time.getHours()}:${(time.getMinutes() + "").padStart(2, "0")}`;
			}
		},
		changeTheme() {
			let colorScheme = document.querySelector(".diary").classList;
			let icon = document.querySelector(".theme-toggle");
			if (colorScheme[1] == "light") {
				colorScheme.replace("light", "dark");
				icon.innerHTML =
					'<svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-sun" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="#52a2c4" fill="none" stroke-linecap="round" stroke-linejoin="round"><desc>Download more icon variants from https://tabler-icons.io/i/sun</desc><path stroke="none" d="M0 0h24v24H0z" fill="none"></path><circle cx="12" cy="12" r="4"></circle><path d="M3 12h1m8 -9v1m8 8h1m-9 8v1m-6.4 -15.4l.7 .7m12.1 -.7l-.7 .7m0 11.4l.7 .7m-12.1 -.7l-.7 .7"></path></svg>';
			} else {
				colorScheme.replace("dark", "light");
				icon.innerHTML =
					'<svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-moon" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="#52a2c4" fill="none" stroke-linecap="round" stroke-linejoin="round"><desc>Download more icon variants from https://tabler-icons.io/i/moon</desc><path stroke="none" d="M0 0h24v24H0z" fill="none"></path> <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z"></path></svg>';
			}

			// document.querySelector("diary").classList.add("dark");
		},
		onInit: () => {
			console.log("lightGallery has been initialized");
		},
		onBeforeSlide: () => {
			console.log("calling before slide");
		},
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "lightgallery/css/lightgallery.css";
@import "lightgallery/css/lg-thumbnail.css";
@import "lightgallery/css/lg-zoom.css";
body {
	font-family: "LXGW WenKai Lite", sans-serif;
}
@font-face {
	font-family: "LXGW WenKai Lite";
	src: url(https://www.stepbystep.cf/assets/LXGWWenKaiLite-Regular.ttf);
}
@font-face {
	font-family: number;
	src: url(https://www.stepbystep.cf/assets/HelveticaNeueLTPro-ThEx.otf);
}
@font-face {
	font-family: Montserrat-Regular;
	src: url(https://www.stepbystep.cf/assets/Montserrat-Regular.otf);
	font-weight: 400;
	font-style: normal;
}
.dark {
	color: #fff;
	background: #121212;
}
.list-desc-text {
	height: 48px;
	line-height: 48px;
}
.light .color-text {
	color: #333;
}
.list-image {
	width: 153px;
	height: 153px;
	object-fit: cover;
	border-radius: 8px;
	background-color: #eee;
}
.q-mr-sm {
	margin-right: 8px;
}
@media screen and (max-width: 700px) {
	.list-image {
		width: 86px;
		height: 86px;
	}
}
.q-px-md {
	padding-left: 16px;
	padding-right: 16px;
}
.column,
.flex,
.row {
	display: flex;
	flex-wrap: wrap;
}
.list-day-text {
	font-size: 26px;
	font-family: Montserrat-Regular;
}
.flex-center,
.justify-center {
	justify-content: center;
}
.flex-center,
.items-center {
	align-items: center;
}
.list-time-text {
	font-size: 14px;
	font-family: Montserrat-Regular;
}
.q-ml-md {
	margin-left: 16px;
}
.q-mt-md {
	margin-top: 16px;
}
.list-content-text {
	font-size: 15px;
	line-height: 180%;
	min-height: 50px;
}
.light .color-desc {
	color: #a2a2a2;
}
.items-baseline {
	align-items: baseline;
}
* {
	font-family: "LXGW WenKai Lite";
}
.light .color-bg-list {
	height: 100%;
	background-color: #fafafa;
}
.dark .color-bg-content {
	background-color: #292828;
}
.fixed,
.fixed-bottom,
.fixed-bottom-left,
.fixed-bottom-right,
.fixed-center,
.fixed-full,
.fixed-left,
.fixed-right,
.fixed-top,
.fixed-top-left,
.fixed-top-right,
.fullscreen {
	position: fixed;
}
.q-card__section--vert {
	padding: 16px;
}
.q-card__section {
	position: relative;
}
header {
	height: 72px;
	text-align: center;
	line-height: 72px;
	box-shadow: 0px 1px 8px 1px rgb(0 0 0 / 7%);
	font-size: 18px;
	z-index: 1000;
}
.q-btn--round .q-btn__wrapper {
	padding: 0;
	min-width: 3em;
	min-height: 3em;
}
.justify-between {
	justify-content: space-between;
}
@media (min-width: 0) {
	.col,
	.col-xs {
		flex: 10000 1 0%;
	}
}
.material-icons {
	font-family: Material Icons;
	font-weight: 400;
	font-style: normal;
	display: inline-block;
	line-height: 1;
	text-transform: none;
	letter-spacing: normal;
	word-wrap: normal;
	white-space: nowrap;
	direction: ltr;
	-webkit-font-smoothing: antialiased;
	text-rendering: optimizeLegibility;
	-moz-osx-font-smoothing: grayscale;
	font-feature-settings: "liga";
}
.list-content {
	max-width: 700px;
	width: 90vw;
	height: auto;
	padding-top: 100px;
	padding-bottom: 140px;
}
.content {
	margin: 0 auto;
}
.list-card {
	border-radius: 8px;
	margin-top: 22px;
	box-shadow: 0px 2px 4px 2px rgb(0 0 0 / 7%);
	padding: 12px 12px 6px 12px;
}
.color-bg-content,
.color-bg-normal {
	background-color: #fff;
}
.absolute-top,
.fixed-top {
	top: 0;
	left: 0;
	right: 0;
}
.q-btn {
	display: inline-flex;
	flex-direction: column;
	align-items: stretch;
	position: relative;
	outline: 0;
	border: 0;
	vertical-align: middle;
	padding: 0;
	font-size: 14px;
	line-height: 1.715em;
	text-decoration: none;
	color: inherit;
	background: transparent;
	font-weight: 500;
	text-transform: uppercase;
	text-align: center;
	width: auto;
	height: auto;
}
.q-focus-helper,
.q-focusable,
.q-hoverable,
.q-manual-focusable {
	outline: 0;
}
.q-btn--round .q-btn__wrapper {
	padding: 0;
	min-width: 3em;
	min-height: 3em;
}
</style>
