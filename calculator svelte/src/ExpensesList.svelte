<script>
    import SectionTitle from "./Title.svelte"
  import Expense from "./Expense.svelte";
    export var expenses = [];
    
</script>

<style>
h2{
  text-transform:capitalize ;
}
</style>

<section>
    <SectionTitle title="Expense List" />
<ul>
    {#each expenses as expense,index} <!--index gives numbering for the sentences also makes them in boxes-->
  <Expense {...expense} _/> <!--this spreads out the properties like name and amount-->
  {:else}
      <h2>currently you have no expenses </h2> <!--this has the value of text being print-->
    {/each}
</ul>
</section>