<script>
export var showForm
</script>


<!-- this is the top search bar whose styling was done by font awesome -->
<nav class="nav">
  <!--this provides links to documents -->
  <div class="nav-center">
    <h1 class="nav-title">budget calculator</h1>
    <button type="button" class="nav-btn" on:click={showForm} > <!--this show form is so when user click the add button the function in app svelte makes the function true which make it run the form-->
      <i class="far fa-plus-square" />
      add item
    </button>
  </div>
</nav>
