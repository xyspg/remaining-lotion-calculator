<script>
	//爱液种类
	const aiye = [
		{ id: 1, name: 'MagicEyes 180mL装', volume: 180 },
		{ id: 2, name: 'Maccos 双子 300mL装', volume: 300 },
		{ id: 3, name: 'G Project 红瓶 220mL装', volume: 220 }
	];
	//定义常量
	let flavours = [];
	let usage = 5;
	let frequency = 1;
	let purchaseDate;
	let customize = 0;
	//计算总容量
	function join(flavours) {
		let sum = 0;
		flavours.forEach((item) => {
			sum += item;
		});
		return sum;
	}
	//计算用完的日期
	function addDays(date, days) {
		let result = new Date(date);
		result.setDate(result.getDate() + days);
		return result.toJSON().toString();
	}
</script>

<body class="bg-gray-100">
	<div class="absolute inset-48 leading-8 opacity-95">
		<h2 class="text-2xl text-center"><ink data-type="blurred">润滑液</ink>计算器</h2>
<!--		动态获取爱液信息-->
		{#each aiye as flavour}
			<label class="flex items-center mb-3 space-x-3">
				<input
					class="form-tick appearance-none bg-white bg-check h-6 w-6 border border-gray-300 rounded-md checked:bg-blue-500 checked:border-transparent focus:outline-none mr-4"
					type="checkbox"
					bind:group={flavours}
					name="flavours"
					value={flavour.volume}
				/>
				{flavour.name}
			</label>
		{/each}
		<p>添加自定义mL</p>
		<input placeholder="添加自定义mL" bind:value={customize} /><br />

		<input type="range" bind:value={usage} min="1" max="20" />
		单次用量：<span>{usage}mL</span><br />
		<input type="range" bind:value={frequency} min="0.25" max="5.0" step="0.25" />
		频率：一天<span>{frequency}</span>次<br />
		购入日期：
		<input type="date" bind:value={purchaseDate} />
<!--		如果没有进行输入则显示prompt，否则显示结果-->
		{#if ((flavours <= 0) & (customize === 0)) | (customize === null)}
			<p>选择你目前拥有的<ink data-type="blurred"> 润滑液 </ink></p>
		{:else}
			<p>
				目前总量：{parseInt(join(flavours)) + parseInt(customize)} mL<br />
				预计在冲 {Math.ceil((parseInt(join(flavours)) + parseInt(customize)) / usage)} 次后耗尽<br
				/>
				{#if purchaseDate != null}
					耗尽日期：<span
						>{addDays(
							purchaseDate,
							Math.ceil((parseInt(join(flavours)) + parseInt(customize)) / usage) / frequency
						).slice(0, 10)}</span
					>
				{/if}
			</p>
		{/if}
	</div>
</body>

<style>
	h2,
	p,
	label {
		font-family: 'PingFang SC', serif;
	}
	ink[data-type='blurred'] {
		filter: blur(4px);
	}

	ink[data-type='blurred']:hover {
		filter: blur(0px);
	}
	label {
		display: flex;
		flex-flow: row nowrap;
	}
</style>
