<template>
	<div class="setting-container">
		<span class="title">{{type}} Setting</span>
		<CloseIcon @click.native="_closeSetting" top="10" right="10" size="24"></CloseIcon>
		<div class="element-container">
			<ul class="element-ul">
				<li class="element-li"
						:style="bgColor"
						v-for="(element, index) in elementList"
						:key="index"
						@click="_selectItem(index, $event)">
					{{element}}
				</li>
			</ul>
		</div>
		<EditorButton
			@click.native="_deleteItem"
			bg-color="#315B96"
			bg-color-hover="#EA5E5E"
			text="Delete"/>
	</div>
</template>

<script>
	import EditorButton from "@/Components/BasicComponents/EditorButton.vue";
	import CloseIcon from "@/Components/BasicComponents/CloseIcon.vue";
	export default {
		name: "ElementSetting",
		components: {CloseIcon, EditorButton},
		props: {
			type: {
				default: "",
				type: String
			},
			elementList: {
				default: null,
				type: Array
			},
		},
		data() {
			return {
				typeColor: '#42B982',
				currentSelectItem: null,
				currentSelectIndex: -1,
			}
		},
		computed: {
			bgColor: function () {
				return {
					background: this.typeColor
				}
			}
		},
		methods: {
			_closeSetting: function () {
				this.$emit("closeElementSetting");
			},
			_selectItem: function (index, event) {
				this.currentSelectIndex = index;
				if (this.currentSelectItem != null) {
					this.currentSelectItem.style.background = this.typeColor;
				}
				this.currentSelectItem = event.target;
				this.currentSelectItem.style.background = "#1C86EE";
			},
			_deleteItem: function () {
				if (this.currentSelectItem != null && this.currentSelectIndex != -1) {
					this.currentSelectItem.style.background = this.typeColor;
					this.elementList.splice(this.currentSelectIndex, 1);
					this.$parent.saveSettings();
				}
			}
		},
		mounted() {
			if (!this.type.includes('PureFunction')) {
				this.typeColor = '#34495D';
			}
		}
	}
</script>

<style lang="less" scoped>
	.setting-container {
		position: relative;
		width: 550px;
		height: 400px;
		background: #ffffff;
		border-radius: 10px;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 30px;
	}
	.title {
		font-size: 18px;
		font-weight: bold;
		color: #333333;
	}
	.element-container {
		width: 80%;
		height: 60%;
		padding: 20px;
		margin: 25px 0;
		background-color: #ffffff;
		box-shadow: 0px 0px 4px 0px #dcdcdc;
		border-radius: 4px;
	}
	.element-ul {
		width: 100%;
		max-height: 100%;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		padding: 0;
		margin: 0;
		overflow-y: scroll;
	}
	.element-li {
		border-radius: 4px;
		font-size: 12px;
		font-weight: bold;
		height: 36px;
		color: #ffffff;
		line-height: 36px;
		padding: 0 10px;
		margin: 5px;
		border: 2px solid transparent;
		cursor: pointer;
	}
	.element-li:hover {
		border: 2px solid #1c86ee;
	}

</style>