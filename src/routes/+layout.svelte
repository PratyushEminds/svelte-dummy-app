<script>
  import "./styles.css";
</script>

<div class="app">
  <h1>Population Records</h1>
  <main>
    <slot />
  </main>
  <footer>
    <p>Population Records<aplication></p>
  </footer>
</div>

<style>
  .app {
    flex-direction: column;
    min-height: 100vh;
  }

  main {
    padding: 1rem;
    width: 100%;
    max-width: 80rem;
    margin: 0 auto;
    box-sizing: border-box;
  }

  footer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 12px;
  }

  footer a {
    font-weight: bold;
  }

  @media (min-width: 480px) {
    footer {
      padding: 12px 0;
    }
  }
</style>
