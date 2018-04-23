<template>
  <div class="treeWrapper">
		<div v-if="node.name">
			<div>
				<button class="btn btn-info" @click="add">+</button>
				<button class="btn btn-danger" @click="remove">-</button>
				<button class="btn btn-primary" @click="edit">{{ node.name }}</button>
				<input type="text" v-model="node.val" v-show="node.name !== 'params' && selected !== 2 && !hasChildren" @blur="blurAutoConvert" class="form-control" :class="{number: selected === 1}">
				<select v-if="node.name !== 'params'" v-model="selected" @change="selectType" :disabled="hasChildren" class="form-control">
					<option v-for="option in options" :value="option.value">{{option.name}}</option>
				</select>
			</div>
		</div>
		<!-- 如果存在 children 的话，循环创建节点 -->
		<tree
			v-if="node.children.length"
			v-for="(child,index) in node.children"
			:key="index"
			:node="child"
			:length="node.children.length">
		</tree>
	</div>
</template>

<script>
export default {
	name: 'tree', // 使用标签时候的名字
	props: ['node', 'length'], // 组件之间传递的值, node 节点信息，length 当前节点 children 的长度，为了样式管理
	data: function() {
		return {
			// 节点绑定的数据类型 0基本数据类型(string number) 1复杂数据类型(object)
			nodeValueDataType: 0,
			selected: 0,
			options: [
				{
					name: 'string',
					value: 0
				},
				{
					name: 'number',
					value: 1
				},
				{
					name: 'object',
					value: 2
				}
			],
			id: 0,
			pid: 0
		}
	},
	computed: {
		// 是否有子节点
		hasChildren() {
			let bool = this.node.children.length > 0
			if (bool) {
				this.selected = 2
			} else {
				this.selected = 0
			}
			return bool
		}
	},
	methods: {
		add: function() {
			var val = prompt('增')
			if (val) {
				if (val.trim() === '') {
					alert('参数key值不能为空')
					return
				}
				var bool = this.node.children.some(function(item, index) {
					return val.trim() !== '' && item.name == val.trim()
				})
				if (bool) {
					alert('兄弟节点参数key值重复')
					return
				} else {
					this.node.children.push({
						name: val,
						val: '',
						children: []
					})
				}
			}
		},
		edit: function() {
			var val = prompt('改')
			if (val) {
				this.node.name = val
			}
		},
		remove: function() {
			var _this = this
			var val = confirm('删')
			if (val) {
				if (this.$parent.node) {
					// 找到上级，循环上级 children 列表，删除选中的
					this.$parent.node.children.forEach(function(item, index) {
						if (item.name == _this.node.name) {
							_this.$parent.node.children.splice(index, 1)
						}
					})
				} else {
					// root 节点
					this.$root.root = {}
				}
			}
		},
		selectType() {
			if (this.selected === 0) {
				// convert to String
				this.node.val = this.node.val.toString()
			} else if (this.selected === 1) {
				// convert to Number
				this.node.val = parseFloat(this.node.val)
			} else if (this.selected === 2) {
				this.node.val = ''
			}
		},
		// 失去焦点 Number类型自动转换
		blurAutoConvert() {
			if (this.selected === 1) {
				this.node.val = parseFloat(this.node.val)
			}
		}
	}
}
</script>
