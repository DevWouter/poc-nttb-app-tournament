<script lang="ts">
  export var id: string;
  export var mode: "info" | "players" | "pool";

  let routes = [
    {
      href: `#/tournament/${id}/info`,
      selected: mode == "info",
      text: "Toernooi",
    },
    {
      href: `#/tournament/${id}/players`,
      selected: mode == "players",
      text: "Spelers",
    },
    {
      href: `#/tournament/${id}/pool`,
      selected: mode == "pool",
      text: "Indeling",
    },
  ];
</script>

<div class="container">
  {#each routes as route}
    <a data-test="page-toggle" class:selected={route.selected} href={route.href}
      >{route.text}</a
    >
  {/each}
</div>

<style>
  .container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    max-width: 600px;
    margin: 16px;
    margin-left: auto;
    margin-right: auto;
  }
  a {
    padding: 8px 16px;
    border: 1px solid var(--nttb-blue);
    text-align: center;
    cursor: pointer;
    text-decoration: none;
    border-right: none;
  }

  a:first-child {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
  }
  a:last-child {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
    border-right: 1px solid var(--nttb-blue);
  }

  .selected {
    color: var(--text-white);
    background-color: var(--nttb-blue);
  }
</style>
