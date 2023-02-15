<script>
    import Title from "./Title.svelte"
    export var name = "";
    export var amount = null;
    export var addExpense;
    export var isEditing;
    export var editExpense;
    export var hideForm;
    /*import {onMount, onDestroy, beforeUpdate, afterUpdate} from 'svelte'
        onMount(()=>{
            console.log('form has mounted')
        });
        beforeUpdate(()=>{
            console.count('before')
        });
        afterUpdate(()=>{
            console.count('after')
        });
        onDestroy(()=>{
            console.log('Destroy')
        }); */


    $: isEmpty = !name || !amount // this is so if any name or amount is empty which is false then the var empty will run and this variable will be true and when we fill name and amount it becomes false

    function handleSubmit () {
        if(isEditing) {
            editExpense({ name, amount });
        }
        else{
            addExpense({ amount, name });  
        }
        name = "";
        amount= null;
        hideForm(); //this is so when we add an expense in the form the form closes automatically
    }
</script>

<section class="form">

    <Title title="add expense"/>
    <form class="expense-form" on:submit|preventDefault={handleSubmit} ><!--the preventDefault makes is so it doesn't refresh when clicking in the submit button-->

        <div class="form-control">
            <label for="name"> name</label>
            <input type="text" id="name" bind:value={name} /> <!--this is for the name-->
        </div>

        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="amount"  bind:value={amount}/> <!--this is for the amount-->
        </div>
        {#if isEmpty}
        <p class="form-empty">please fill out all form fields</p><!--the if is so when both amount and name have given values the paragraph will be removed-->
        {/if}
    
                                                    <!--and the class disabled is so that until empty is true the global css disabled will work which makes it grey-->
        <button type="submit" class="btn btn-block" class:disabled={isEmpty} disabled={isEmpty}> <!--the disabled is so that as long as empty is true the button wont work-->
        {#if isEditing} edit expense{:else}add expense{/if}<!--this is the submit button-->
        </button> <!--this is if so when we are editing a expense the button name will change-->

        <button type="button" class="close-btn" on:click={hideForm}><!--hide is so when we click on the close button it makes the form open false and the form closes-->
            <i class="fas fa-times" /> <!--this is the close button-->
            close
        </button>
    </form>
</section>