<script lang="ts">
	import { spring } from 'svelte/motion';

	let initialSeconds = 10; // Valor inicial do timer
	let seconds = initialSeconds; // Tempo atual do timer
	let inputSeconds = initialSeconds; // Valor para o input
	let timerId: NodeJS.Timeout | null = null;
	const displayed_time = spring();

	$: displayed_time.set(seconds);

	let isRunning = false; // Controla o estado do timer

	function startTimer() {
		if (timerId) return; // Se já estiver em execução, não inicia outro

		isRunning = true; // Marca que o timer está em execução
		timerId = setInterval(() => {
		seconds -= 1;
		}, 1000);
	}

	function resetTimer() {
		stopTimer(); // Para o timer caso esteja rodando
		seconds = initialSeconds; // Redefine para o valor inicial
		startTimer(); // Inicia uma nova contagem
	}

	function stopTimer() {
		if (timerId) {
			clearInterval(timerId);
			timerId = null;
			isRunning = false; // Marca que o timer não está em execução
		}
	}

	function clearTimer() {
		stopTimer(); // Para o timer caso esteja rodando
		seconds = initialSeconds; // Limpa o contador
	}

	function setTimer(value: number) {
		initialSeconds = value; // Atualiza o valor inicial
		inputSeconds = value; // Atualiza o valor do input
		seconds = value; // Atualiza o valor do temporizador
		stopTimer(); // Para o timer caso esteja rodando
	}


	function modulo(n: number, m: number) {
		return ((n % m) + m) % m;
	}
</script>

<div class="timer">
	<div class="timer-display">
		<strong>{seconds}</strong> <!-- Exibe o temporizador em texto grande -->
	</div>

	<label for="timer-input">Configure o tempo em segundos:</label>
	<input
		type="number"
		min="1"
		bind:value={inputSeconds} 
		aria-label="Set timer in seconds"
		class="timer-input"
		on:input={() => setTimer(inputSeconds)} 
	/>

	<div>
		<button class="start" on:click={isRunning ? resetTimer : startTimer} aria-label={isRunning ? "Reset timer" : "Start timer"}>
			{isRunning ? 'Reset' : 'Start'}
		</button>
		{#if isRunning}
			<button class="stop" on:click={stopTimer} aria-label="Stop timer">
				Stop
			</button>
		{/if}

		{#if !isRunning && seconds !== initialSeconds}
		<button class="clear" on:click={clearTimer} aria-label="Clear timer">
			Clear
		</button>
		{/if}
	</div>
</div>

<style>
	.timer {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 1rem 0;
	}
	.timer label {
		font-size: 1rem; /* Tamanho do texto da label */
		color: #f1f1f1;
		margin-bottom: 0.5rem; /* Espaço entre a label e o input */
	}

	.timer-display {
		font-size: 12rem; /* Tamanho do texto do temporizador */
		color: var(--color-theme-1);
		margin-bottom: 1rem; /* Espaço entre a exibição e o input */
	}

	.timer-input {
		width: 4em;
		margin-bottom: 1rem; /* Espaço entre o input e os botões */
		font-size: 1rem; /* Tamanho discreto do input */
		text-align: center;
		background-color: transparent;
		border: 1px solid #444; /* Adicione uma borda para melhor visibilidade */
		color: var(--color-theme-1);
		appearance: none; /* Remove o estilo padrão do input */
		border-radius: 0.5rem;
	}

	.timer-input:focus {
		outline: none; /* Remove o contorno do foco */
	}

	.timer button {
		padding: 1rem 2rem;
		border: 1px solid #444;
		background-color: #c2c2c2;
		color: #fff;
		cursor: pointer;
		border: none;
		border-radius: 5px;
	}
	.timer button.start {
		background-color: #4caf50;
	}
	.timer button.stop {
		background-color: #f44336;
	}
	.timer button.clear {
		background-color: #ff9800;
	}
</style>
