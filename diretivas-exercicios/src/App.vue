<template>
	<div id="app">
		<h1>Diretivas</h1>
		<p v-destaque="'pink'">Testando diretiva personalizada</p>
		<p v-destaque:fundo.atrasar="'pink'">Testando diretiva atrasada</p>
		<p v-destaque:fundo="'pink'">Testando diretiva personalizada</p>
		<p v-destaque:fundo="'pink'">Testando diretiva com varios modificadores</p>
		<p v-destaque.atrasar="cor">Testando diretiva atrasada 2</p>
		<p v-destaque:fundo="cor">Testando diretiva personalizada</p>
		<hr>
		<h1>Diretivas locais</h1>
		<p v-destaque-local:fundo.atrasar.alternar="{cor1: 'green', cor2: 'red', atraso: 2000, intervalo: 200}">Testando diretiva com objeto</p>
		<!--<p v-teste:argumento.mod1.mod2.mod3="binding.value"></p>-->

	</div>
</template>

<script>
export default {
	data() {
		return {
			cor: 'cyan'
		}
		
	},
	directives: {
		'destaque-local': {
			bind(el, binding, vnode) {
				const aplicarCor = cor => {
					if (binding.arg === 'fundo') {
						el.style.background = cor
					} else {
						el.style.color = cor
					}
				}

				let atraso = 0
				if (binding.modifiers['atrasar']) {
					atraso = binding.value.atraso
				}

				
				const cor1 = binding.value.cor1
				const cor2 = binding.value.cor2
				let corAtual = cor1

				setTimeout(() => {
					if (binding.modifiers['alternar']) {
						setInterval( () => {
							corAtual = corAtual === cor1 ? cor2 : cor1
							aplicarCor(corAtual)
						}, binding.value.intervalo)
					} else {
						aplicarCor(binding.value.cor1)
					}
				}, atraso)
			}
		}
	}
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	font-size: 2.5rem;
}
</style>
