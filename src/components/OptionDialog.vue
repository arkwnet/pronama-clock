<template>
	<div class="dialog">
		<div class="header">設定</div>
		<div class="contents">
			<div class="option">
				<div class="columns">
					<div class="left">
						テーマ
					</div>
					<div class="right">
						<select v-model="option.theme" @change="changeOption">
							<option v-for="theme in optionThemes" :key="theme.name" :value="theme.name">{{ theme.name }}</option>
						</select>
						<div class="preview" :style="preview">
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="button" @click="closeDialog()">閉じる</div>
	</div>
</template>

<script>
export default {
	name: "App",
	components: {},
	props: {
		currentOption: {
			type: Object,
			"default": () => (this.option)
		}
	},
	emits: ["closeDialog", "changeOption"],
	data() {
		return {
			option: {
				"theme": "default"
			},
			optionThemes: [],
			preview: ""
		}
	},
	mounted: function() {
		this.option = this.currentOption;
		this.axios.get(`./config.json?timestamp=${new Date().getTime()}`).then((response) => {
			let optionThemes = [];
			for (let i = 0; i < response.data.themes.length; i++) {
				optionThemes.push({name: response.data.themes[i]});
			}
			this.optionThemes = optionThemes;
			this.loadPreview();
		}).catch((e) => {
			console.log(e);
		});
	},
	methods: {
		closeDialog: function() {
			this.$emit("closeDialog");
		},
		changeOption: function() {
			this.$emit("changeOption", this.option);
			this.loadPreview();
		},
		loadPreview: function() {
			this.axios.get(`./assets/themes/${this.option.theme}/theme.json?timestamp=${new Date().getTime()}`).then((response) => {
				this.preview = "background-image: url('assets/themes/" + this.option.theme + "/" + response.data.image + "')";
			}).catch((e) => {
				console.log(e);
			});
		}
	}
}
</script>

<style scoped lang="scss">
@import "../assets/sass/Dialog.scss";
@import "../assets/sass/OptionDialog.scss";
</style>