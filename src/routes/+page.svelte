<script>
	import { goto } from '$app/navigation';
    import { base } from '$app/paths';

	let question = null;
	let value = '';
	let operator = '+';
	let difficulty = 50;

	let numbers_between = null;


	function TwoNumbers(difficulty) {
	  // We generate two random numbers
	  // with size based on the difficulty.
	  // Note: difficulty in [0, 1]

	  // The maximum difficulty is with numbers of size max_zeros
	  // const min_num = Math.floor(Math.pow(10, 3 * difficulty));
	  const min_num = 0;
	  const max_num = Math.floor(Math.pow(10, 3 * difficulty + 0.5));

	  numbers_between = [min_num, max_num];
	  console.log('The numbers lie between ' + min_num + ' and ' + max_num);

	  const num1 = Math.floor(Math.random() * (max_num - min_num)) + min_num;
	  const num2 = Math.floor(Math.random() * (max_num - min_num)) + min_num;
	  return [num1, num2];
	}


	function makeSum(operator, difficulty) {
	  let nums = TwoNumbers(difficulty)
	  const s = nums[0] + operator + nums[1];
	  return s;
	}


  function newQuestion() {
    const sum = makeSum(operator, difficulty / 100);
    question = sum;
  }

  function changeOperator(o) {
    operator = o;
    newQuestion();
  }

  function addNumber(num) {
    console.log(value + num, eval(question))
    if (value + num === eval(question).toString()) {
      value = '';
      newQuestion();
    } else {
      value = value + num;
    }

  }

  function clearNumber() {
    value = '';
  }


  newQuestion();
</script>

<svelte:head>
	<title>mathmatica</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="py-10">
	<div class="max-w-md mx-auto px-4">


	<div class="grid grid-cols-3 gap-x-2 mb-5">
		<button class="px-3 py-1 text-black font-medium text-xl bg-gray-200 w-full shadow-button rounded-md active:shadow-none" on:click={() => changeOperator('+')}>+</button>
		<button class="px-3 py-1 text-black font-medium text-xl bg-gray-200 w-full shadow-button rounded-md active:shadow-none" on:click={() => changeOperator('-')}>-</button>
		<button class="px-3 py-1 text-black font-medium text-xl bg-gray-200 w-full shadow-button rounded-md active:shadow-none" on:click={() => changeOperator('*')}>*</button>
	</div>

	<div class="mb-20">
		<label for="default-range" class="block mb-1 text-sm font-medium text-white">Difficulty {difficulty} <span class="text-xs">(numbers between {numbers_between[0]},{numbers_between[1]})</span></label>
		<input id="default-range" type="range" bind:value={difficulty} on:input={() => newQuestion()} class="w-full h-2 bg-gray-700 accent-gray-200 rounded-lg appearance-none cursor-pointer">
	</div>


	<div class="text-7xl text-center text-white font-semibold mb-14">
	  <div>{question}={value ? value : '?'}</div>
	</div>

	<div class="max-w-md space-y-3">
	  <div class="grid grid-cols-3 gap-x-3">
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('1')}>1</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('2')}>2</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('3')}>3</button>
	  </div>
	  <div class="grid grid-cols-3 gap-x-3">
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('4')}>4</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('5')}>5</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('6')}>6</button>
	  </div>
	  <div class="grid grid-cols-3 gap-x-3">
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('7')}>7</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('8')}>8</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('9')}>9</button>
	  </div>
	  <div class="grid grid-cols-3 gap-x-3">
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-sky-600 w-full shadow-button rounded-md" on:click={() => addNumber('0')}>0</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-gray-400 w-full shadow-button rounded-md" on:click={() => addNumber('-')}>-</button>
	    <button class="py-4 text-white font-medium text-3xl px-3 bg-orange-500 w-full shadow-button rounded-md" on:click={() => clearNumber()}>CE</button>
	  </div>
	</div>

<!-- 
	{#each teams as team, team_index}
	<div class="border-b py-5">
		<div class="text-white text-xl font-semibold">Team {team.id}</div>

		<div class="mt-3 divide-y rounded-md overflow-hidden">
			{#each team.players as player}
				<div class="">
					<input type="text" bind:value={player} class="py-3 px-4 block w-full text-gray-900">
				</div>
			{/each}
		</div>

		<div>
			<button on:click={() => add_player(team_index)} class="mt-3 py-2 font-medium text-lg bg-gray-200 w-full shadow-button rounded-md active:shadow-none text-sm">Add player</button>
		</div>
	</div>
	{/each}


	<div class="">
		<button on:click={() => add_team()} class="mt-5 py-3 text-white font-medium text-lg px-3 bg-sky-500 w-full shadow-button rounded-md active:shadow-none text-sm">Add team</button>
	</div>

	<div class="mt-5">
		<button on:click={() => ready()} class="py-3 text-white font-medium text-lg px-3 bg-sky-600 w-full shadow-button rounded-md active:shadow-none">Ready!</button>
	</div>


	<div class="text-center mt-10">
		<button on:click={() => reset_game_state()} class="text-white text-sm">Reset game state</button>
	</div> -->


	</div>
</section>

