<template>
	<div v-if="store" class="guide changelogAurora">
		<p class="title">
			<NotesIcon />
			Changelog
		</p>
		<!-- eslint-disable-next-line vue/no-v-html -->
		<div v-html="changelogStore"></div>
		<div class="note">
			<p>
				View the full release
				<a href="https://gitlab.com/AuroraOSS/AuroraStore/-/releases" target="_blank" rel="noopener">here</a>
			</p>
		</div>
	</div>
	<div v-else-if="droid" class="guide changelogAurora">
		<p class="title">
			<NotesIcon />
			Changelog
		</p>
		<!-- eslint-disable-next-line vue/no-v-html -->
		<div v-html="changelogDroid"></div>
		<div class="note">
			<p>
				View the full release
				<a href="https://gitlab.com/AuroraOSS/AuroraDroid/-/releases" target="_blank" rel="noopener">here</a>
			</p>
		</div>
	</div>
	<div v-else-if="warden" class="guide changelogAurora">
		<p class="title">
			<NotesIcon />
			Changelog
		</p>
		<!-- eslint-disable-next-line vue/no-v-html -->
		<div v-html="changelogWarden"></div>
		<div class="note">
			<p>
				View the full release
				<a href="https://gitlab.com/AuroraOSS/AppWarden/-/releases" target="_blank" rel="noopener">here</a>
			</p>
		</div>
	</div>
	<div v-else-if="walls" class="guide changelogAurora">
		<p class="title">
			<NotesIcon />
			Changelog
		</p>
		<!-- eslint-disable-next-line vue/no-v-html -->
		<div v-html="changelogWalls"></div>
		<div class="note">
			<p>
				View the full release
				<a href="https://gitlab.com/AuroraOSS/AuroraWallpapers/-/releases" target="_blank" rel="noopener">
					here
				</a>
			</p>
		</div>
	</div>
</template>

<script>
import marked from "marked";

export default {
	props: {
		store: {
			type: Boolean,
			default: false,
		},
		droid: {
			type: Boolean,
			default: false,
		},
		warden: {
			type: Boolean,
			default: false,
		},
		walls: {
			type: Boolean,
			default: false,
		},
	},

	data() {
		return {
			changelogStore: "Failed to load data from server.",
			changelogDroid: "Failed to load data from server.",
			changelogWarden: "Failed to load data from server.",
			changelogWalls: "Failed to load data from server.",
		};
	},

	async mounted() {
		try {
			const { data } = await this.$store.dispatch("getStoreStableReleaseData");
			this.$data.changelogStore = marked(data.changelog.body).replace(
				/(?<=\(|(, ))@(.*?)(?=\)|(, ))/g,
				"<a href='https://gitlab.com/$2' target='_blank' rel='noopener'>@$2</a>"
			);
		} catch (e) {
			console.error(e);
		}
		try {
			const { data } = await this.$store.dispatch("getDroidStableReleaseData");
			this.$data.changelogDroid = marked(data.changelog.body).replace(
				/(?<=\(|(, ))@(.*?)(?=\)|(, ))/g,
				"<a href='https://gitlab.com/$2' target='_blank' rel='noopener'>@$2</a>"
			);
		} catch (e) {
			console.error(e);
		}
		try {
			const { data } = await this.$store.dispatch("getWardenReleaseData");
			this.$data.changelogWarden = marked(data.changelog.body).replace(
				/(?<=\(|(, ))@(.*?)(?=\)|(, ))/g,
				"<a href='https://gitlab.com/$2' target='_blank' rel='noopener'>@$2</a>"
			);
		} catch (e) {
			console.error(e);
		}
		try {
			const { data } = await this.$store.dispatch("getWallsReleaseData");
			this.$data.changelogWalls = marked(data.changelog.body).replace(
				/(?<=\(|(, ))@(.*?)(?=\)|(, ))/g,
				"<a href='https://gitlab.com/$2' target='_blank' rel='noopener'>@$2</a>"
			);
		} catch (e) {
			console.error(e);
		}
	},
};
</script>

<style lang="stylus">
.changelogAurora
	.title
		text-align center
	div
		white-space
		h3
			font-size 1.1rem
</style>
