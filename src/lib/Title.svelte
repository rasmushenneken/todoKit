<script>
  import "../css/variables.css"
</script>

<h1>
  <slot />
</h1>

<style>
  h1 {
    color: var(--color-heading);
    font-size: var(--size-heading);
    width: 100%;
    animation: flyIn 500ms forwards ease-in-out;
  }
</style>
