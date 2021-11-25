<template>
	<div tabindex="0" class="body">
		<div class="game">
			<div :key="key" v-for="(arr, key) of matrix" class="horizontal">
				<div
					:key="key"
					v-for="(item, key) of arr"
					:class="['item', `val-${item}`]"
				>
					{{ item || '' }}
				</div>
			</div>
		</div>
		<button
			@click="
				up()
				randomPlacement()
			"
			class="up"
		>
			上
		</button>
		<div class="left-and-right">
			<button
				@click="
					left()
					randomPlacement()
				"
				class="left"
			>
				左
			</button>
			<button
				@click="
					right()
					randomPlacement()
				"
				class="right"
			>
				右
			</button>
		</div>
		<button
			@click="
				down()
				randomPlacement()
			"
			class="down"
		>
			下
		</button>
	</div>
</template>

<script>
// 创建一个矩阵用来存放数据
const matrix = []
for (let i = 0; i < 4; i++) {
	matrix[i] = new Array(4).fill(0)
}

export default {
	name: 'GameApp',
	data() {
		return {
			matrix,
		}
	},
	methods: {
		// 向上方法
		up() {
			const recursive = (x, y) => {
				if (x === 0) return
				if (matrix[x - 1][y] === 0) {
					matrix[x - 1].splice(y, 1, matrix[x][y])
					matrix[x].splice(y, 1, 0)
					recursive(x - 1, y)
				} else if (matrix[x - 1][y] === matrix[x][y]) {
					matrix[x - 1].splice(y, 1, matrix[x][y] * 2)
					matrix[x].splice(y, 1, 0)
					recursive(x - 1, y)
				}
			}
			for (let i = 1; i < 4; i++) {
				for (let j = 0; j < 4; j++) {
					recursive(i, j)
				}
			}
		},

		// 向下方法 与向上相反
		down() {
			const recursive = (x, y) => {
				if (x === 3) return
				if (matrix[x + 1][y] === 0) {
					matrix[x + 1].splice(y, 1, matrix[x][y])
					matrix[x].splice(y, 1, 0)
					recursive(x + 1, y)
				} else if (matrix[x + 1][y] === matrix[x][y]) {
					matrix[x + 1].splice(y, 1, matrix[x][y] * 2)
					matrix[x].splice(y, 1, 0)
					recursive(x + 1, y)
				}
			}
			for (let i = 2; i >= 0; i--) {
				for (let j = 0; j < 4; j++) {
					recursive(i, j)
				}
			}
		},

		// 向左方法
		left() {
			const recursive = (x, y) => {
				if (y === 0) return
				if (matrix[x][y - 1] === 0) {
					matrix[x].splice(y - 1, 1, matrix[x][y])
					matrix[x].splice(y, 1, 0)
					recursive(x, y - 1)
				} else if (matrix[x][y - 1] === matrix[x][y]) {
					matrix[x].splice(y - 1, 1, matrix[x][y] * 2)
					matrix[x].splice(y, 1, 0)
					recursive(x, y - 1)
				}
			}
			for (let i = 0; i < 4; i++) {
				for (let j = 1; j < 4; j++) {
					recursive(i, j)
				}
			}
		},

		// 向右方法
		right() {
			const recursive = (x, y) => {
				if (y === 3) return
				if (matrix[x][y + 1] === 0) {
					matrix[x].splice(y + 1, 1, matrix[x][y])
					matrix[x].splice(y, 1, 0)
					recursive(x, y + 1)
				} else if (matrix[x][y + 1] === matrix[x][y]) {
					matrix[x].splice(y + 1, 1, matrix[x][y] * 2)
					matrix[x].splice(y, 1, 0)
					recursive(x, y + 1)
				}
			}
			for (let i = 0; i < 4; i++) {
				for (let j = 2; j >= 0; j--) {
					recursive(i, j)
				}
			}
		},
		/*
		 * 随机位置生成一个数字 1
		 *
		 * 遍历为0的位置，生成hash表并记录为0的位置
		 * 计算hash数组长度
		 * 通过hash数组长度生成随机数
		 * 将hash数组的第随机数位置的值修改为1
		 * */
		randomPlacement() {
			const hash = {}
			let count = 0
			for (let i = 0; i < 4; i++) {
				for (let j = 0; j < 4; j++) {
					if (this.matrix[i][j] === 0) {
						hash[count] = {
							i,
							j,
						}
						count++
					}
				}
			}
			const random = Math.floor(Math.random() * count)
			this.$set(this.matrix[hash[random].i], hash[random].j, 1)
		},
		// 根据键盘按的上下左右方向键分别调用上面的 up down left right 函数
		keyPress(e) {
			switch (e.key) {
				case 'ArrowUp':
					this.up()
					break
				case 'ArrowDown':
					this.down()
					break
				case 'ArrowLeft':
					this.left()
					break
				case 'ArrowRight':
					this.right()
					break
			}
			this.randomPlacement()
		},
	},
	mounted() {
		document.addEventListener('keyup', this.keyPress)
		for (let i = 0; i < 4; i++) {
			this.randomPlacement()
		}
	},
	destroyed() {
		document.removeEventListener('keyup', this.keyPress)
	},
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.body {
	width: 100%;
	height: 100%;
	display: flex;
	flex-flow: column nowrap;
	align-items: center;
}

.horizontal {
	display: flex;
	flex-flow: row nowrap;
}
.item {
	width: 50px;
	height: 50px;
	margin: 5px;
	border: #d7d7d7 1px solid;
	display: flex;
	align-items: center;
	justify-content: center;
}

.game {
	margin-bottom: 20px;
}

button {
	width: 50px;
	height: 50px;
	font-weight: bold;
}

.left-and-right .right {
	margin-left: 50px;
}

.val-1 {
	font-weight: 600;
	color: #272a2f;
}

.val-2 {
	font-weight: 600;
	color: blue;
}

.val-4 {
	font-weight: 600;
	color: rebeccapurple;
}

.val-8 {
	font-weight: 600;
	color: saddlebrown;
}

.val-16 {
	font-weight: 800;
	color: blue;
}

.val-32 {
	font-weight: 800;
	color: rebeccapurple;
}

.val-64 {
	font-weight: 800;
	color: saddlebrown;
}

.val-128 {
	font-weight: 1000;
	color: blue;
}

.val-256 {
	font-weight: 1000;
	color: rebeccapurple;
}

.val-512 {
	font-weight: 1000;
	color: saddlebrown;
}
</style>
