<script>
  import { location } from "svelte-spa-router";

  export let loggedIn = false;
  console.log(loggedIn);

  const handleLogout = () => {
    localStorage.clear();
    window.location.href = "/";
  };
</script>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="/#/">AASK</a>
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link" aria-current="page" href="/#/">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/#/about">About</a>
        </li>
        {#if loggedIn}
          <li class="nav-item">
            <a class="nav-link" href="/#/blogs">My Blogs</a>
          </li>
        {/if}
      </ul>

      <div class="mx-2">
        {#if !loggedIn}
          <button
            class="btn btn-outline-primary"
            data-bs-toggle="modal"
            data-bs-target="#loginModal">Login</button
          >
          <button
            class="btn btn-outline-primary"
            data-bs-toggle="modal"
            data-bs-target="#signupModal">Signup</button
          >
        {:else}
          <button on:click={handleLogout} class="btn btn-outline-danger"
            >Logout</button
          >
        {/if}
        {#if $location === "/blogs"}
          <button
            class="btn btn-outline-primary"
            data-bs-toggle="modal"
            data-bs-target="#blogPostModal">Create post</button
          >
        {/if}
      </div>
    </div>
  </div>
</nav>
