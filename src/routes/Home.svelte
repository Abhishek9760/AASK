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

        let newD = Object.values(data);
        let uids = Object.keys(data);
        let finalData = [];

        newD.map((item, index) => {
          let newItem = Object.values(item);
          let postIds = Object.keys(item);
          let userid = uids[index];

          newItem.map((postData, ind) => {
            let newData = { ...postData, key: postIds[ind], uid: userid };
            finalData.push(newData);
          });
        });

        blogPosts = finalData;
      })
      .catch(() => (isLoading = false));
  };

  if (token) {
    fetchData();
  }
</script>

<div class="container my-4">
  {#if token !== null}
    <h1 class="display-1 my-3">All Blogs</h1>
  {/if}
  {#if token === null}
    <div
      id="carouselExampleCaptions"
      class="carousel slide"
      data-bs-ride="carousel"
    >
      <div class="carousel-indicators">
        <button
          type="button"
          data-bs-target="#carouselExampleCaptions"
          data-bs-slide-to="0"
          class="active"
          aria-current="true"
          aria-label="Slide 1"
        />
        <button
          type="button"
          data-bs-target="#carouselExampleCaptions"
          data-bs-slide-to="1"
          aria-label="Slide 2"
        />
        <button
          type="button"
          data-bs-target="#carouselExampleCaptions"
          data-bs-slide-to="2"
          aria-label="Slide 3"
        />
      </div>
      <div class="carousel-inner carousel-fade">
        <div class="carousel-item active">
          <img src="/images/2.jpg" class="d-block w-100" alt="..." />
          <div class="carousel-caption d-none d-md-block">
            <h2>Welcome to AASK</h2>
            <p>Technology, Marketing and Trends</p>
          </div>
        </div>
        <div class="carousel-item">
          <img src="/images/1.jpg" class="d-block w-100" alt="..." />
          <div class="carousel-caption d-none d-md-block">
            <h2>The only blog site you need</h2>
            <p>Technology, Marketing and Trends</p>
          </div>
        </div>
        <div class="carousel-item">
          <img src="images/3.jpg" class="d-block w-100" alt="..." />
          <div class="carousel-caption d-none d-md-block">
            <h2>Easy to use</h2>
            <p>Technology, Marketing and Trends</p>
          </div>
        </div>
      </div>
      <button
        class="carousel-control-prev"
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide="prev"
      >
        <span class="carousel-control-prev-icon" aria-hidden="true" />
        <span class="visually-hidden">Previous</span>
      </button>
      <button
        class="carousel-control-next"
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide="next"
      >
        <span class="carousel-control-next-icon" aria-hidden="true" />
        <span class="visually-hidden">Next</span>
      </button>
    </div>
  {/if}
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
