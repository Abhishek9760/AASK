<script>
  export let description;
  export let createdAt;
  export let title;
  export let tag;
  export let thumbnail;
  export let key;
  export let uid;
</script>

<div class="col-md-6">
  <div
    class="row g-0 border rounded overflow-hidden flex-md-row mb-4 shadow-sm h-md-250 position-relative"
  >
    <div class="col p-4 d-flex flex-column position-static">
      <div class="mb-2 badge bg-danger" style="width:min-content">{tag}</div>
      <h3 class="mb-0">{title}</h3>
      <div class="mb-1 text-muted">{new Date(createdAt).toDateString()}</div>
      <p class="card-text text-truncate" style="max-width: 150px;">
        {description}
      </p>
      <a href="/#/blog/{key}?userId={uid}" class="stretched-link btn-link"
        >Read more</a
      >
    </div>
    <div class="col-auto d-none d-lg-block">
      <img
        src={thumbnail}
        style="object-fit: cover;"
        alt="event name"
        class="bd-placeholder-img"
        width="250"
        height="250"
      />
    </div>

  </div>
</div>
