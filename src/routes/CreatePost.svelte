<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const sayHello = () => {
    dispatch("message", {
      text: "Hello!",
    });
  };

  let title = "";
  let description = "";
  let imageUrl = "";
  let tag;

  const tags = {
    1: "Science",
    2: "Technology",
    3: "Marketing",
  };

  const handleSubmit = () => {
    if (tag === "0") {
      return;
    }
    let token = localStorage.getItem("token");
    let uid = localStorage.getItem("userId");

    fetch(
      `https://college-blog-3a380-default-rtdb.firebaseio.com/blogs/${uid}.json?auth=${token}`,
      {
        method: "POST",
        body: JSON.stringify({
          createdAt: new Date().toISOString(),
          description: description,
          title: title,
          tag: tags[tag],
          thumbnail: imageUrl,
        }),
      }
    ).then((res) => {
      sayHello();
    });
  };
</script>

<div
  class="modal fade"
  id="blogPostModal"
  tabindex="-1"
  aria-labelledby="blogPostModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="blogPostModalLabel">Create Blog Post</h5>
        <button
          type="button"
          class="btn-close"
          data-bs-dismiss="modal"
          aria-label="Close"
        />
      </div>
      <div class="modal-body">
        <form on:submit|preventDefault={handleSubmit}>
          <div class="mb-3">
            <label for="title" class="form-label">Title</label>
            <input
              required
              bind:value={title}
              type="text"
              class="form-control"
              id="title"
              aria-describedby="blogTitle"
            />
          </div>
          <div class="mb-3">
            <label for="description" class="form-label">Description</label>
            <textarea
              required
              bind:value={description}
              id="description"
              class="form-control"
            />
          </div>
          <div class="mb-3">
            <label for="image" class="form-label">Image URL</label>
            <input
              required
              bind:value={imageUrl}
              type="text"
              class="form-control"
              id="image"
              aria-describedby="blogTitle"
            />
          </div>
          <div class="mb-3">
            <select
              class="form-select"
              aria-label="Default select example"
              bind:value={tag}
            >
              <option selected value="0">Choose tag</option>
              <option value="1">{tags[1]}</option>
              <option value="2">{tags[2]}</option>
              <option value="3">{tags[3]}</option>
            </select>
          </div>
          <button data-bs-dismiss="modal" type="submit" class="btn btn-primary"
            >Add</button
          >
        </form>
      </div>
    </div>
  </div>
</div>
