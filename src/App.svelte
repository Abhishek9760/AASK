<script>
  import Router from "svelte-spa-router";
  import Navbar from "./components/Navbar.svelte";
  import About from "./routes/About.svelte";
  import Blog from "./routes/Blog.svelte";
  import Blogs from "./routes/Blogs.svelte";
  import Home from "./routes/Home.svelte";
  import Login from "./routes/Login.svelte";
  import SignUp from "./routes/SignUp.svelte";
  import NotFound from "./routes/NotFound.svelte";
  import { initializeApp } from "firebase/app";

  $: loggedIn = localStorage.getItem("token") !== null ? true : false;

  let routes = {
    "/": Home,
    "/about": About,
    "/blogs": Blogs,
    "/blog/:id": Blog,

    "*": NotFound,
  };
  const firebaseConfig = {
    apiKey: "AIzaSyDuXxiR0eKFvoaiRGYJx_gdmR-2YUu4HvQ",
    authDomain: "college-blog-3a380.firebaseapp.com",
    databaseURL: "https://college-blog-3a380-default-rtdb.firebaseio.com",
    projectId: "college-blog-3a380",
    storageBucket: "college-blog-3a380.appspot.com",
    messagingSenderId: "335786660187",
    appId: "1:335786660187:web:c23f0c4bb53c82aaeaca7e",
    measurementId: "G-0R7D5V1J9L",
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);

  const handleLogin = () => {
    loggedIn = true;
  };
</script>

<main>
  <Navbar {loggedIn} />
  <Login on:loggedin={handleLogin} />
  <SignUp />
  <Router {routes} />
</main>
