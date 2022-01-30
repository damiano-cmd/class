<script>
	import "boxicons";

	let list = {};
	let error = "";

	function hexColor() {
		let string = "#";
		for (let i = 0; i < 3; i++) {
			let color = Math.floor(Math.random() * 200).toString(16);
			if (color.length < 2) {
				color += "0";
			}
			string += color;
		}
		return string
	}

	let text = "";
	function add(e) {
		e.preventDefault();
		if (Object.keys(list).includes(text)) {
			err("You have this item in your todo list!");
		} else {
			list[text] = [false, hexColor()];
			text = "";
		}
	}
	function deleteItem(index) {
		delete list[index];
		list = {...list};
	}
	function err(tex) {
		error = tex;
		setTimeout(() => {
			error = "";
		}, 2000);
	}
	function update(tex) {
		text = tex;
		deleteItem(tex);
	}
</script>

<main>
	<div class="todo">
		<form on:submit={add}>
			<input type="text" placeholder="Todo" bind:value={text}>
			<button type="submit">Add</button>
		</form>
		<p>{error}</p>
		<div class="list">
			{#each Object.keys(list).reverse() as i}
				<label 
					style={"background-color: " + list[i][1]}
					>
					<input type="checkbox" bind:checked={list[i][0]} >
					<p>{i}</p>
					<span class="ed">
						<button
							on:click={() => {update(i)}}
							>
							<box-icon name='edit'></box-icon>
						</button>
						<div class="div"></div>
						<button 
							on:click={() => {deleteItem(i)}} 
							>
							<box-icon name='message-square-x'></box-icon>
						</button>
					</span>
				</label>
			{/each}
		</div>
	</div>
</main>

<style>
	main {
		width: 100%;
		height: 100%;

		display: flex;
		justify-content: center;
		align-items: center;

		background: linear-gradient(225deg, rgba(1,0,255,1) 0%, rgba(0,255,169,1) 100%);
	}
	.todo {
		background: #222;

		display: flex;
		flex-direction: column;
		align-items: center;

		padding: 2rem;
		border-radius: 1rem;
	}


	.todo form {
		display: flex;
		flex-wrap: nowrap;
		margin: 1rem 0;

		width: 100%;

		background-color: blueviolet;
		border-radius: 3px;

		background: linear-gradient(270deg, rgba(250,0,255,1) 0%, rgba(130,0,255,1) 26%, rgba(74,0,255,1) 100%);
	}
	.todo form input {
		width: 100%;

		background-color: #222;

		margin: 2px;
		padding: .5rem 1rem;
		outline: none;

		border-radius: 3px;
	}
	.todo form button {
		padding: 0 1rem;
		background: none;
	}


	.list {
		display: flex;
		flex-direction: column;
		align-items: center;

		width: 100%;
		max-height: 20rem;

		overflow-y: auto;
	}
	.list label {
		width: calc(100% - .8rem);

		padding: .4rem;
		border-radius: 3px;
		margin: .4rem 0;

		display: flex;
		align-items: center;
	}
	.list label input {
		position: absolute;
		top: 0;

		width: 0;
		height: 0;

		opacity: 0;
	}
	.list label input:checked ~ p {
		text-decoration: line-through;
		color: #999;
	}
	.list label .ed {
		width: auto;
		height: 100%;

		padding: 0;
		margin-left: auto;

		display: flex;
		align-items: center;
	}
	.list label .ed button {
		background: none;
	}
	.list label .ed .div {
		width: 2px;
		height: 1.25rem;

		margin: 0 .25rem;

		background-color: #fff;
	}
	.list label .ed box-icon {
		fill: #fff;
	}
</style>