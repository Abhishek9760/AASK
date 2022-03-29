<script>
  export let comment;
  export let author;
  export let uid;
  export let postid;
  export let id;
  export let fetchComments;

  let userId = localStorage.getItem("userId");
  let token = localStorage.getItem("token");

  const handleDeleteComment = () => {
    deleteComment();
  };

  const deleteComment = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/comments/${postid}/${id}.json?auth=${token}`,
      { method: "DELETE" }
    )
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        fetchComments();
      });
  };
</script>

<blockquote class="blockquote">
  <div class="d-flex justify-content-between">
    <p class="mb-2">
      {comment}
    </p>
    {#if userId === uid}
      <button
        type="button"
        class="btn btn-outline-danger btn-sm"
        on:click={handleDeleteComment}>Delete</button
      >
    {/if}
  </div>
  <footer class="blockquote-footer">
    By <cite title="Source Title">{author}</cite>
  </footer>
</blockquote>
