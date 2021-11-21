<script>
  import BlogItem from "../components/BlogItem.svelte";
  import Spinner from "../components/Spinner.svelte";

  let token = localStorage.getItem("token");
  $: blogPosts = [];
  $: isLoading = true;

  const fetchData = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        isLoading = false;
        let uids = Object.keys(data);
        data = Object.values(data);
        blogPosts = data.map(function (item, index) {
          return {
            key: Object.keys(item)[0],
            // uid
            uid: uids[index],
            ...item[Object.keys(item)[0]],
          };
        });
      })
      .catch(() => (isLoading = false));
  };

  if (token) {
    fetchData();
  }
</script>

<div class="container my-4">
  <h1 class="display-1 my-3">All Blogs</h1>
  <div class="row mb-2">
    {#if token === null}
      <h1 class="display-1">Welcome to AASK Blogs</h1>
      <p class="lead">Please login to make your blogs</p>
    {:else if isLoading && token !== null}
      <Spinner />
    {:else}
      {#each blogPosts as blog}
        <BlogItem {...blog} />
      {/each}
    {/if}
  </div>
</div>
