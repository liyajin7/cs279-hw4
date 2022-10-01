<script>
	import { getApp, getApps, initializeApp } from "firebase/app";
	import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, addDoc } from "firebase/firestore";
	import { firebaseConfig } from "$lib/firebaseConfig";
	// import { amp, browser, dev } from '$app/env';
	// import { browser } from "$app/environment";

	// initialize Firebase app
	const firebaseApp = (getApps().length === 0 ? initializeApp(firebaseConfig) : getApp());

	// get our stored database from Firestore
	const db = getFirestore();

	// create a reference to our database collection
	const colRef = collection(db, "todos2")

	// declare empty todos array
	let todos = [];

	// populates our todos array with data from the database using onSnapshot
	const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
			// (FOR DEBUGGING)
			// console.log("onsnapshot running?", (querySnapshot));
			// if (querySnapshot.empty) {
            //     console.log('No matching documents.');
            //     return;
            // }
			let fbTodos = [];
			querySnapshot.forEach((doc) => {
				let todo = {...doc.data(), id: doc.id}
				fbTodos = [todo, ...fbTodos];
			});
			console.table(fbTodos);
			todos = fbTodos;
		});

	// instantiate array of to-do's & empty task
	let task = "";
	let error = "";

	// function to create a new todo list item
	const addTodo = async () => {
		// let todo = { task: task, isComplete: false, createdAt: new Date() };
		// only adds task (via "Enter" keypress) if entry is nonempty, resets error
		if (task !== "") {
			error = "";
			const docRef = await addDoc(collection(db, "todos2"), {
				task: task,
				isComplete: false,
				createdAt: new Date()
			});
		}
		else { error = "Cannot add an empty task :("; }
		// reset task in the input bar
		task = "";
	};

	// async function to mark as todos as complete; awaits document (app) update via
	// user checking / unchecking a to-do. called later on check button's click.
	const markTodoAsComplete = async (item) => {
		await updateDoc(doc(db, "todos2", item.id), {
			isComplete: !item.isComplete
		});
		// OLD CODE: todos[index].isComplete = !todos[index].isComplete;
	}

	// async function to delete todos; awaits document (app) update via  user clicking
	// the delete button. called later on delete button's click.
	const deleteTodo = async (id) => {
		await deleteDoc(doc(db, "todos2", id));
		// OLD CODE: 
			// let deleteItem = todos[index];
			// todos = todos.filter((item) => item != deleteItem);
	}

	// function called when key is pressed: allows user to add to-do by pressing enter after
	// typing into input field
	const keyIsPressed = (event) => {
		if (event.key === "Enter") {
			addTodo();
		}
	}
</script>


<!-- truly, truly sorry for the horrendous styling but it is egregiously late -->
<div style="font-family: Avenir; align:justify">
<h2>Liya's Lovely To-do's 🕊 (albeit unstyled) </h2>
<p style="font-size: 15px; color: green; font-style: italic">Click the ✅ icon to complete to-do's. <br> Click the 🗑️ icon to delete to-do's.</p>

<!-- input for to-dos, binds user value to task & adds todo when button is clicked -->
<input type="text" style="margin: 20px  10px 3px 6px; font-family: Avenir; font-size: 15px" placeholder="Add a task" bind:value={task}/>
<button style="font-family: Avenir; font-size: 15px; background-color: green; color: white" on:click={addTodo}>Add</button>

<ul>
	<!-- for each object in "todo" task, set "complete" styling to be dependeng
	on isComplete attribute of todo object -->
    {#each todos as item, index}
        <li class:complete={item.isComplete}>
			<span style="margin: 5px">
				{item.task}
			</span>

			<!-- button to mark task as complete -->
			<span>
				<button on:click={() => markTodoAsComplete(item)}>✅</button>
			</span>

			<!-- button to delete task -->
			<span>
				<button on:click={() => deleteTodo(item.id)}>🗑️</button>
			</span>
		</li>

		<!-- display alternate message if no to-do's found -->
		{:else}
		<p style="font-style: italic; text-justify: left">No to-do's found!</p>
	{/each}
	<p class="error">{error}</p>
</ul>
</div>

<!--  -->
<svelte:window on:keydown={keyIsPressed}/>

<style>
	.complete {
		text-decoration: line-through
	}
	.error {
		color: red;
	}
</style>