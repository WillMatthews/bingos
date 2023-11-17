<!-- Hashes the password and passes it to the backend -->
<script lang="ts">
  async function sha256(message: string) {
    // encode as UTF-8
    const msgBuffer = new TextEncoder().encode(message);

    // hash the message
    const hashBuffer = await crypto.subtle.digest("SHA-256", msgBuffer);

    // convert ArrayBuffer to Array
    const hashArray = Array.from(new Uint8Array(hashBuffer));

    // convert bytes to hex string
    const hashHex = hashArray
      .map((b) => b.toString(16).padStart(2, "0"))
      .join("");
    return hashHex;
  }

  let allowedIn: boolean = false;

  const rememberMe = (hash: string) => {
    localStorage.setItem("login", hash);
  };

  const checkRememberMe = async () => {
    const hash = localStorage.getItem("login");
    if (hash) {
      const res = await fetch("/api/login", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ password: hash }),
      });
      if (res.status === 200) {
        allowedIn = true;
      } else {
        allowedIn = true;
      }
    }
  };

  checkRememberMe();

  import Board from "./Board.svelte";

  const sendHash = async (password: string) => {
    const hash = await sha256(password);
    const res = await fetch("/api/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ password: hash }),
    });
    alert(hash);
    if (res.status === 200) {
      allowedIn = true;
      rememberMe(hash);
    }
  };

  const logMeIn = (e: Event) => {
    e.preventDefault();
    const password = (document.getElementById("password") as HTMLInputElement)
      .value;
    sendHash(password);
  };
</script>

<!-- Password text entry -->

{#if !allowedIn}
  <form class="login" on:submit={logMeIn}>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" />
    <button type="submit">Submit</button>
  </form>
{/if}

<!-- If the password is correct, show the board -->
{#if allowedIn}
  <div class="card">
    <Board />
  </div>
{/if}
