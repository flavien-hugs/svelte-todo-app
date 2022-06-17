<script>
	// import components

	import TodoItem from './TodoItem.svelte';

	let todos = [
		{
			id: 1,
			title: 'My first todo',
			completed: false
		},
		{
			id: 2,
			title: 'My second todo',
			completed: false
		},
		{
			id: 1,
			title: 'My third todo',
			completed: false
		}
	]

	// declared datas

	let newTodoTitle = '';
	let currentFilter = 'all';
	let nextId = 4;

	// add logo

	let src = 'img/favicon.png'

	// computed properties

    $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
    $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'completed'
        ? todos.filter(todo => todo.completed)
        : todos.filter(todo => !todo.completed)

    // function event todo
	
	const addTodo = (event) => {
		if (event.key == 'Enter') {
			todos = [...todos, {
				id: nextId,
				completed: false,
				title: newTodoTitle
			}];

			nextId = nextId + 1;
			newTodoTitle = "";
		}
	}

	const checkAllTodos = (event) => {
		todos.forEach(todo => todo.completed = event.target.checked);
		todos = todos;
	}

	const updateFilter = (newFilter) => {
		currentFilter = newFilter;
	}

	const clearComplted = () => {
		todos = todos.filter(todo => !todo.completed);
	}

	const handleDeleteTodo = (event) => {
		todos = todos.filter(todo => todo.id !== event.detail.id);
	}

	const handleToggleComplete = (event) => {
		const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);

		const updatedTodo = {
			...todos[todoIndex],
			completed: !todos[todoIndex].completed
		};

		todos = [
			...todos.slice(0, todoIndex),
			updatedTodo,
			...todos.slice(todoIndex + 1),
		];
	}
</script>

<div class="container">
	<a href="https://www.unsta.net/" target="_blank">
		<img src={src} alt="svelte-logo" class="logo">
	</a>

	<h2>Application Svelte Todo</h2>

	<input
		type="text"
		class="todo-input"
		placeholder="Insert todo item ..."
		bind:value={newTodoTitle}
		on:keydown={addTodo}>

	{#each filteredTodos as todo }
		<div class="todo-item">
			<TodoItem
				{...todo}
				on:deleteTodo={handleDeleteTodo}
				on:toggleComplete={handleToggleComplete}
			/>
		</div>
	{/each}

	<div class="inner-container">
		<div>
			<label>
				<input
					class="inner-container-input"
					type="checkbox"
					on:change={checkAllTodos}>
					Tout cocher
			</label>
		</div>

		<div>{todosRemaining} éléments restants</div>
	</div>

	<div class="inner-container">
		<div>
			<button
				on:click={() => updateFilter('all')}
				class:active="{currentFilter === 'all'}"
			>Tous</button>

			<button
				on:click={() => updateFilter('active')}
				class:active="{currentFilter === 'active'}"
			>Actif</button>

			<button
				on:click={() => updateFilter('completed')}
				class:active="{currentFilter === 'completed'}"
			>Terminé</button>
		</div>

		<div>
			<button
				on:click={clearCompleted}
			>Effacer terminé</button>
		</div>
	</div>
</div>

<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
    }
    .logo {
        display: block;
        margin: 20px auto;
        width: 30%;
        height: 30%;
    }
    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
    }
    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 15px;
        margin-bottom: 13px;
    }
    .inner-container-input {
        margin-right: 12px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }
    button:hover {
        background: lightseagreen;
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightseagreen;
    }
</style>