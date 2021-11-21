<script>
  import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

  let email = "";
  let password = "";

  const signUp = (email, password) => {
    const auth = getAuth();
    createUserWithEmailAndPassword(auth, email, password)
      .then((userCredential) => {
        const user = userCredential.user;
        console.log(user);
      })
      .catch((error) => {
        const errorCode = error.code;
        const errorMessage = error.message;
        console.log(errorCode);
      });
  };

  const handleSubmit = () => {
    console.log(email, password);
    signUp(email, password);
  };
</script>

<!-- Modal -->
<div
  class="modal fade"
  id="signupModal"
  tabindex="-1"
  aria-labelledby="signupLabel"
  aria-hidden="true"
>
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="signupModalLabel">Get an AASK Account</h5>
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
            <label for="exampleInputEmail1" class="form-label"
              >Email address</label
            >
            <input
              bind:value={email}
              type="email"
              class="form-control"
              id="exampleInputEmail1"
              aria-describedby="emailHelp"
            />
            <div id="emailHelp" class="form-text">
              We'll never share your email with anyone else.
            </div>
          </div>
          <div class="mb-3">
            <label for="exampleInputPassword1" class="form-label"
              >Password</label
            >
            <input
              bind:value={password}
              type="password"
              class="form-control"
              id="exampleInputPassword1"
            />
          </div>
          <!-- <div class="mb-3">
            <label for="exampleInputPassword1" class="form-label"
              >Confirm-Password</label
            >
            <input
              type="password"
              class="form-control"
              id="cexampleInputPassword1"
            />
          </div> -->

          <button data-bs-dismiss="modal" type="submit" class="btn btn-primary"
            >Create Account</button
          >
        </form>
      </div>
    </div>
  </div>
</div>
