<template>
	<transition name="cover">
		<div class="cover" id="cover" v-if="isCover"></div>
	</transition>
	<div class="button" id="night_mode" @click="changeNightMode"><img :src="nightModeImage"></div>
	<div class="button" id="option" @click="openOptionDialog"><img :src="optionImage"></div>
	<video class="video" id="video" loop>
		<source :src="video" type="video/mp4">
	</video>
	<div class="main" :style="style">
		<Clock @openVersionDialog="openVersionDialog" @openUpdateAlert="openUpdateAlert" />
	</div>
	<transition name="dialog">
		<UpdateAlert v-if="isUpdateAlert" @click="reload" />
	</transition>
	<transition name="cover">
		<div class="cover" id="dialog_cover" v-if="isDialogCover" @click="closeDialog"></div>
	</transition>
	<transition name="dialog">
		<VersionDialog v-if="isVersionDialog" @closeDialog="closeDialog" />
	</transition>
	<transition name="dialog">
		<OptionDialog v-if="isOptionDialog" :currentOption="this.option" @closeDialog="closeDialog" @changeOption="changeOption" />
	</transition>
</template>

<script src="./assets/js/App.js">
</script>

<style scoped lang="scss">
@import "./assets/sass/App.scss";
</style>