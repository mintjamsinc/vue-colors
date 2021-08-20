<!-- Copyright (c) 2021 MintJams Inc. Licensed under MIT License. -->

<template>
	<div class="p-1 position-relative rounded d-flex flex-wrap">
		<div v-for="c in colors" :key="c.value" class="icon-block d-flex justify-content-center align-items-center">
			<span class="color-icon flat-control rounded-circle shadow-sm c-pointer" :class="additionalClasses(c)" v-on:click="onClick(c)"></span>
		</div>
	</div>
</template>

<script>
import commons from '@mintjamsinc/webtop-app-commons';
const {Objects} = commons;

export default {
	props: {
		'multiple': {
			'type': Boolean,
			'default': false
		},
	},
	data() {
		return {
			'colors': [],
			'selectedColors': [],
			'ui': null,
		};
	},
	created() {
		let vm = this;
		class UI {
			get colors() {
				return vm.colors;
			}
			set colors(colors) {
				if (Array.isArray(colors)) {
					vm.colors = Objects.clone(colors);
				} else {
					vm.colors = [colors];
				}
			}

			get selection() {
				if (vm.multiple) {
					return vm.selectedColors;
				}
				if (vm.selectedColors.length == 0) {
					return '';
				}
				return vm.selectedColors[0];
			}
			set selection(values) {
				let a;
				if (Array.isArray(values)) {
					a = Objects.clone(values);
				} else {
					a = [values];
				}
				let availables = [];
				for (let c of this.colors) {
					availables.push(c.value);
				}
				let l = [];
				for (let e of a) {
					if (availables.indexOf(e) == -1) {
						continue;
					}
					l.push(e);
				}
				vm.selectedColors = l;
			}

			set onChanged(f) {
				this.$onChanged = f;
			}
		}
		vm.ui = new UI();
	},
	methods: {
		isSelected(color) {
			let vm = this;
			return (vm.selectedColors.indexOf(color.value) != -1);
		},
		additionalClasses(color) {
			let vm = this;
			let l = [];
			l.push(color['class']);
			if (!vm.isSelected(color) && color.value == '') {
				l.push('border');
			}
			if (vm.isSelected(color)) {
				l.push('active');
			}
			return l;
		},
		onClick(color) {
			let vm = this;
			if (vm.multiple) {
				let i = vm.selectedColors.indexOf(color.value);
				if (i == -1) {
					vm.selectedColors.push(color.value);
				} else {
					vm.selectedColors.splice(i, 1);
				}
			} else {
				if (vm.selectedColors.indexOf(color.value) == -1) {
					vm.selectedColors = [color.value];
				}
			}
		}
	},
	watch: {
		'selectedColors': function() {
			let vm = this;
			vm.ui.$onChanged();
		},
	},
}
</script>

<style lang="scss" scoped>
.icon-block {
	width: 2.5rem;
	height: 2.5rem;
	min-width: 2.5rem;
	min-height: 2.5rem;
}
</style>
