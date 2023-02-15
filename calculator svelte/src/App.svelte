			<!--all the btn , icons are in global css and in fontawesome-->

<script>
	import {setContext, onMount,afterUpdate } from "svelte"; // onmount selects all the expenses

	// components 
		import Navbar from "./Navbar.svelte";
		import ExpensesList from "./ExpensesList.svelte";
		import Totals from"./Totals.svelte";
		import ExpenseForm from "./ExpenseForm.svelte";
		import Modal from "./Modal.svelte";

		// data
		//import expensesData from "./expenses";


    //variables
	var expenses = [];


	//set editing variables
	var setName = "";
	var setAmount = null;
	var setId = null;

	//toggle form variables
	var isFormOpen = false; //this is so we can hide the form until we click on add item

	//reactive
	$: isEditing = setId ? true : false;

	$: total = expenses.reduce((acc,curr)=>{ //acc is the value 0 which has curr which is the amount getting added to it 
		return (acc += curr.amount) // this is the amount returned at the end
	},0) // value used at bottom

	//functions 
	function showForm() {
		isFormOpen = true;
	}

	function hideForm(){
		isFormOpen = false;
		setName = '';
		setAmount = null;
		setId = null; //the set's are so when we close the form while editing the values are deleted
		
	}


	function removeExpense(id){ //this function has the data of expenses so only with this we can remove . its functions are in expenses.svelte
		expenses = expenses.filter(item => item.id !== id); // this is to check is the id matches with the users clicked ids so we can delete it properly

	}
		function clearExpenses(){
			expenses=[] //this is the function that works when clicking the clear expense button 
				
		}
	
		function addExpense({ name, amount }) {
			var expense = {id: Math.random() * Date.now()
			, name, amount }; // all of this is user input multiplied by date  and then added to the list
			expenses = [expense, ...expenses]; // this makes it so that if we add a new expense it is added at top
				
		} // the ... spreads out the data

		function setModifiedExpense(id) {
			var expense = expenses.find(item => item.id === id); //this makes it so that this function will find the id of the item the user targets

			setId = expense.id;
			setName = expense.name;
			setAmount = expense.amount;
			showForm(); //this is so when we edit it automatically opens the form
			
		} // this function also adds value to the variable's above like id the real id to the var id

		function editExpense({name,amount}){
			expenses = expenses.map(item =>{
				return item.id === setId? {...item, name, amount } : {...item}
			})
			setId = null;
			setAmount = null;
			setName = "";
			
		}

		


	//context 
setContext ('remove',removeExpense)// this re send it so the deleted expense is not there and removed
setContext ('modify',setModifiedExpense)

// local storage
function setLocalStorage(){
	localStorage.setItem("expenses",JSON.stringify //the JSON converts a java script value to json sting so we can put it in local storage
	(expenses));
}
onMount(()=>{
	expenses = localStorage.getItem("expenses")
	? JSON.parse(localStorage.getItem("expenses")) //json parse converts the string back to js so we can put it in website 
	
	:[]
});
afterUpdate(()=>{
	console.log("")
	setLocalStorage(); //update is so it uploads the data after any change to expenses
}
)
</script>


	<Navbar {showForm} /><!-- this sets it so that it understands that we are trying html-->
<main class="content">

	{#if isFormOpen}
	<Modal>
	<ExpenseForm {addExpense} name = {setName} amount = {setAmount} {isEditing} {editExpense} {hideForm} /> <!-- the add expense is so user can add their expense-->
    </Modal> <!--the modal here is to make the form like a pop up window-->
	{/if} <!--the if is so that the form open variable isn't true it wont show the form-->

	<Totals title="total expenses" {total} /> <!--the total in the end is to represent the value of the variable above -->

	<ExpensesList {expenses} /> <!--we add remove so when we delete an item it'll be taken from the list-->

	<button type="button" class="btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button> <!--this is the button to clear all the expenses-->
</main>