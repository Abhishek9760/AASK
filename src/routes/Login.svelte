<script>
  import { getAuth, signInWithEmailAndPassword } from "firebase/auth";
  import { createEventDispatcher } from "svelte";

  let dispatch = createEventDispatcher();

  let email = "";
  let password = "";

  const signIn = (email, password) => {
    const auth = getAuth();
    signInWithEmailAndPassword(auth, email, password)
      .then((userCredential) => {
        const user = userCredential.user;
        localStorage.setItem("token", user.accessToken);
        localStorage.setItem("userId", user.uid);
        localStorage.setItem("expiresIn", user.stsTokenManager.expirationTime);
        dispatch("loggedin");
        window.location.href = "/";
      })
      .catch((error) => {
        const errorCode = error.code;
        const errorMessage = error.message;
      });
  };

  const handleSubmit = () => {
    signIn(email, password);
  };
</script>

<!-- Modal -->
<div
  class="modal fade"
  id="loginModal"
  tabindex="-1"
  aria-labelledby="loginModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="loginModalLabel">Login to AASK</h5>
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
            <label for="email" class="form-label">Email address</label>
            <input
              required
              bind:value={email}
              type="email"
              class="form-control"
              id="email"
              aria-describedby="emailHelp"
            />
            <div id="emailHelp" class="form-text">
              We'll never share your email with anyone else.
            </div>
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input
              required
              bind:value={password}
              type="password"
              class="form-control"
              id="password"
            />
          </div>

          <button data-bs-dismiss="modal" type="submit" class="btn btn-primary"
            >Login</button
          >
        </form>
      </div>
    </div>
  </div>
</div>
