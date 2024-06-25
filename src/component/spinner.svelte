<script>
  export let size = 40;
</script>

<div class="spinner" style="--spinner-size: {size}px;"></div>

<style>
  .spinner {
    border: 8px solid rgba(19, 185, 55, 0.1);
    border-left-color: rgb(31, 170, 31);
    border-radius: 50%;
    width: var(--spinner-size);
    height: var(--spinner-size);
    animation: spin 1s linear infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
</style>
