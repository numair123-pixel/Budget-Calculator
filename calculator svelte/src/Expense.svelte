<script>
    import { getContext } from "svelte"; // this is for deleting th expenses
    export var id;
    export var name = ""; // this take the value from expense.js
    export var amount = 0;
    var displayAmount = false;//so we have to click on the dropdown box to see the amount 

    function toggleAmount (){
        displayAmount = !displayAmount; // this makes it so that if its false it'll become true and its used in the button below
    }
    const removeExpense = getContext('remove'); // this removes the expense
    const setModifiedExpense = getContext('modify');
</script>

    <article class="single-expense"><!--this makes it so the expense is displayed like a box-->

    <div class="expense-info">

        <h2>{name} 
            <button class="amount-btn" on:click={toggleAmount}> <!--the on basically controls how an element works so it can be used as a event listener-->
            <i class="fas fa-caret-down"></i><!--this makes the drop down icon-->
            </button>
        </h2>

        {#if displayAmount}

        <h4> amount : ${amount}</h4> <!--this displays the amount-->

        {/if} <!--make so it until th var displayAmount isn't true wont show the amount-->

    </div>

    <div class="expense-buttons"> <!--this is the button for the pen next to expenses-->
        <button class="epxense-btn edit-btn" on:click="{()=>setModifiedExpense(id)}">
        <i class="fas fa-pen" />
    </button>

    <button class="epxense-btn delete-btn" on:click={() => removeExpense(id)}><!--remove is used here to delete a expense -->
        <i class="fas fa-trash" /><!--this has the icon of trash-->
    </button>
    
    </div> 
    </article>