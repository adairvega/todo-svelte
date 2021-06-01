<script>
	
	import { Toast } from 'bootstrap';

	let toastEl;

	let opciones = {texto: '', color: ''};

	const mostrarToast = (texto, color) => {
		opciones = {texto, color};
		new Toast(toastEl).show();
	}

	let todos = [];	
	let todo = {id: '', texto: '', estado: false};

	if(localStorage.getItem("todos")){
		todos = JSON.parse(localStorage.getItem("todos"));
	}

	$: localStorage.setItem("todos", JSON.stringify(todos));
	

	const addTodo = () => {
		// todos.push(todo);
		if (!todo.texto.trim()) {
			console.log('Texto vacio');
			todo.texto = '';
			return;
		}
		todo.id = Date.now();
		todos = [...todos, todo];
		console.log(todos);
		todo = {id: '', texto: '', estado: false};

		mostrarToast('Agregado correctamente', 'bg-primary');
	}

	const deleteTodo = (id) => {
		todos = todos.filter(item => item.id !== id);
		mostrarToast('Eliminado correctamente', 'bg-danger');
	}

	const editTodo = id => {
		todos = todos.map(
			item => item.id === id ? 
			{...item, estado: !item.estado} :
			item
		);
		mostrarToast('Modificado correctamente', 'bg-warning');
	}

	const classIcono = valor => (
		valor ? 'bi bi-arrow-clockwise' : 'bi bi-check2'
	)
	
	const classEstado = valor => (
		valor ? 'btn-success' : 'btn-warning'
	)
</script>

<div class="container">
	<h1 class="display-5 my-3">CRUD</h1>
	<form on:submit|preventDefault={addTodo}>
		<input
			type="text"
			placeholder="Enter para agregar todo"
			class="form-control shadow border-0"
			bind:value={todo.texto}
		>
	</form>
	{#each todos as item}
		<div class="shadow my-3 p-3 lead">
			<p class={item.estado ? 'text-decoration-line-through': ''}>{item.texto}</p>
			<button class="btn btn-sm {classEstado(item.estado)}" on:click={editTodo(item.id)}>
				<i class={classIcono(item.estado)}></i>
			</button>
			<button class="btn btn-sm btn-danger" on:click={deleteTodo(item.id)}>
				<i class="bi bi-trash"></i>
			</button>
		</div>
	{/each}
	<div class="toast-container position-absolute p-3 top-0 end-0">
		<div bind:this={toastEl} class="toast align-items-center text-white {opciones.color} border-0" role="alert" aria-live="assertive" aria-atomic="true">
			<div class="d-flex">
			  <div class="toast-body">
				{opciones.texto}
			  </div>
			  <button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast" aria-label="Close"></button>
			</div>
		</div>
	</div>
</div>

<style>
</style>