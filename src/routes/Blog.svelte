<script>
  import { pop, querystring } from "svelte-spa-router";
  import Spinner from "../components/Spinner.svelte";
  export let params = {};
  let post = {};
  let isLoading = true;
  let uid = $querystring.split("=")[1];

  const go_home = () => {
    pop();
  };

  const fetchPost = () => {
    let token = localStorage.getItem("token");
    // let uid = localStorage.getItem("userId");
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs/${uid}/${params.id}.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        post = data;
        isLoading = false;
      });
  };

  fetchPost();
</script>

<div class="container blog-detail">
  {#if isLoading}
    <Spinner />
  {:else}
    <button
      on:click={go_home}
      class="btn btn-secondary btn-sm"
      style="margin-block: 1rem;">Go Back</button
    >
    <div class="row">
      <div class="col-12" style="max-height: 300px;overflow:hidden;">
        <img
          src={post.thumbnail}
          alt={post.title}
          class="img-fluid"
          style="height: 100%;"
        />
      </div>
      <div class="col-12">
        <h1 class="display-1">{post.title}</h1>
        <h4>{new Date(post.createdAt).toDateString()}</h4>
        <span class="badge bg-secondary">{post.tag}</span>
        <p class="lead">{post.description}</p>
      </div>
    </div>
  {/if}
</div>

<!-- <button>Back</button> -->
