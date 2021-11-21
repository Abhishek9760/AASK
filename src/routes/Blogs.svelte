<script>
  import BlogItem from "../components/BlogItem.svelte";
  import CreatePost from "./CreatePost.svelte";
  import Spinner from "../components/Spinner.svelte";

  $: blogPosts = [];
  $: isLoading = true;

  const handleMessage = (event) => {
    isLoading = true;
    fetchData();
  };
  const fetchData = () => {
    let token = localStorage.getItem("token");
    let uid = localStorage.getItem("userId");
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs/${uid}.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        isLoading = false;
        let keys = Object.keys(data);
        let values = Object.values(data);
        blogPosts = values.map(function (item, index) {
          return {
            key: keys[index],
            // userId: uid,
            ...item,
          };
        });
      })
      .catch(() => (isLoading = false));
  };

  fetchData();
</script>

<CreatePost on:message={handleMessage} />
<div class="container my-4">
  <div class="row mb-2">
    {#if isLoading}
      <Spinner />
    {:else}
      {#each blogPosts as blog}
        <BlogItem {...blog} />
      {/each}
    {/if}
  </div>
</div>
