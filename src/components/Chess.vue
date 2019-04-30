<template>
	<form>
		<input type="text" v-model="input"/>
		<button type="button" v-on:click="onClick">Вычислить</button>
		<p>{{res}}
		<p/>

	</form>
</template>

<script>
	export default {
		name: 'Chess',
		data: function () {
			const ERROR_EMPTY = "Введите значение";
			const ERROR_INCORREST = "Введите коретное значение вида \"h2\"";
			//Проверяет является ли значение правильным значением на шахматной доске.
			var validInput = function (val) {
				val = val.toString();
				if (val == "")
					return ERROR_EMPTY;
				if (
					val.length != 2 ||
					val[0].toUpperCase() < "A" ||
					val[0].toUpperCase() > "H" ||
					+val[1] < 1 ||
					+val[1] > 8
				)
					return ERROR_INCORREST;
				return "";
			}
			return {
				input: "a1",
				res: "",
				//Получение всех возможных ходов конем
				getHorseMoves: function (val) {
					var error = validInput(val);
					if (error != "")
						return error;
					var letter = val[0];
					var num = val[1];
					var res = [];
					// Перебор всех возможных комбинаций ячеек.
					for (var i = -2; i < 3; i++) {
						for (var j = -2; j < 3; j++) {
							if (i != 0 && j != 0 && Math.abs(i) != Math.abs(j)) {
								var char_code = letter.charCodeAt(0) + i;
								var _letter = String.fromCharCode(char_code);
								var _num = +num + j;
								var f = _letter + _num;
								if (validInput(f) == "")
									res.push(f);
							}
						}
					}
					return res.join("; ");
				}
			};
		},
		methods:
			{
				onClick: function () {
					var val = this.input.trim();
					this.res = this.getHorseMoves(val);
				}
			}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h3 {
		margin: 40px 0 0;
	}

	ul {
		list-style-type: none;
		padding: 0;
	}

	li {
		display: inline-block;
		margin: 0 10px;
	}

	a {
		color: #42b983;
	}
</style>
