<script>
  import BlogItem from "../components/BlogItem.svelte";
  import Spinner from "../components/Spinner.svelte";

  let token = localStorage.getItem("token");
  $: blogPosts = [];
  $: isLoading = true;

  const handleMessage = (event) => {
    isLoading = true;
    fetchData();
  };
  const fetchData = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        isLoading = false;
        data = Object.values(data);
        blogPosts = data.map(function (item, index) {
          return {
            key: Object.keys(item)[0],
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
  <div class="row mb-2">
    {#if token === null}
      <p>Please login</p>
    {:else if isLoading && token !== null}
      <Spinner />
    {:else}
      {#each blogPosts as blog}
        <BlogItem {...blog} />
      {/each}
    {/if}
  </div>
</div>
