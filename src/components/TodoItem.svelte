<script>
	import { settings, todos } from "../stores.js";
	import { fade } from "svelte/transition";

	export let index;
	export let todo;

	function remove(index) {
		let tempTodos = $todos;
		tempTodos.splice(index, 1);
		$todos = tempTodos;
		localStorage.setItem("todos", JSON.stringify($todos));
	}

	function check(index) {
		//let tempTodos = $todos;
		//tempTodos[index].completed = !tempTodos[index].completed;
		//$todos = tempTodos;
		todo.completed = !todo.completed;
	}
</script>

<div 
	class="my-4 px-0 md:px-16 py-8 bg-indigo-300 flex items-center flex-col"
	transition:fade={{ duration: 100 }} 
>
	<input 
		type="text"
		bind:value={todo.title}
		class="bg-transparent text-center text-indigo-800 font-medium text-2xl transition focus:outline-none"
		class:completed={$settings.completionStyle && todo.completed}
	/>
	{#if $settings.description}
		<div bind:clientHeight={todo.height} style="height: {todo.height}px;" class="todo_textarea">
			<textarea
				class="bg-transparent text-center font-light text-indigo-800 resize-none focus:outline-none"
				bind:value={todo.description}
			/>
		</div>
	{/if}

	<button on:click={() => remove(todo.index)} class="text-red-500">x</button>

	{#if $settings.completionStyle}
		<button on:click={() => check(todo.index)} class="text-black">✅</button>
	{/if}
</div>

<style type="postcss">
	button {
		@apply text-2xl focus:outline-none;
	}
	.completed {
		@apply line-through;
	}
	/* Why doesn't this work in class? */
	.todo_textarea {
		@apply resize-y overflow-hidden w-48 md:w-auto;
	}
</style>
