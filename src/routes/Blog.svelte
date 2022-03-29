<script>
  import { pop, querystring } from "svelte-spa-router";
  import Comment from "../components/Comment.svelte";
  import Spinner from "../components/Spinner.svelte";
  export let params = {};
  let token = localStorage.getItem("token");
  let displayName = localStorage.getItem("displayName");
  let post = {};
  let isLoading = true;
  let uid = $querystring.split("=")[1];
  let storedUid = localStorage.getItem("userId");
  let postid = params.id;
  let commentText;
  let comments = [];

  const fetchPost = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs/${uid}/${postid}.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        post = data;
        isLoading = false;
      });
  };

  const deletePost = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs/${uid}/${postid}.json?auth=${token}`,
      { method: "DELETE" }
    )
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        pop();
      });
  };

  const handleAddComment = () => {
    if (commentText.trim().length > 0) {
      postComment(commentText);
      fetchComments();
      commentText = "";
    }
  };

  const postComment = (comment) => {
    let data = {
      userid: storedUid,
      text: comment,
      displayName,
    };
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/comments/${postid}.json?auth=${token}`,
      {
        method: "POST",
        body: JSON.stringify(data),
      }
    );
  };

  const fetchComments = () => {
    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/comments/${postid}.json?auth=${token}`
    )
      .then((res) => res.json())
      .then((data) => {
        let commentIds = Object.keys(data);
        let commentData = Object.values(data);
        comments = commentData.map((item, index) => {
          return {
            ...item,
            id: commentIds[index],
          };
        });
      });
  };

  fetchPost();
  fetchComments();
</script>

<div class="container blog-detail">
  {#if isLoading}
    <Spinner />
  {:else}
    <button
      on:click={pop}
      class="btn btn-secondary btn-sm"
      style="margin-block: 1rem;">Go Back</button
    >
    {#if uid === storedUid}
      <button type="button" class="btn btn-danger btn-sm" on:click={deletePost}
        >Delete</button
      >
    {/if}
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
        <h1 class="display-4">{post.title}</h1>
        <h4>{new Date(post.createdAt).toDateString()}</h4>
        <span class="badge bg-secondary">{post.tag}</span>
        <p class="lead">{post.description}</p>
      </div>
      <hr size="3" />
      <div class="col-12">
        <h1 class="display-4">Comments</h1>
        {#each comments as c}
          <Comment
            {postid}
            {fetchComments}
            id={c.id}
            uid={c.userid}
            comment={c.text}
            author={c.displayName}
          />
        {/each}
      </div>

      <div class="col-12">
        <textarea
          bind:value={commentText}
          placeholder="Write your comment..."
          class="form-control"
          aria-label="With textarea"
          rows="5"
        />
        <button
          on:click={handleAddComment}
          class="btn btn-secondary btn-sm my-2 mb-5"
          type="button">Add</button
        >
      </div>
    </div>
  {/if}
</div>
