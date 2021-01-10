<script lang="ts">
  import TournamentHeader from "../_Header.svelte";
  import PoolRanking from "./_PoolRanking.svelte";

  import { findTournamentById } from "../../../data/tournament";
  import { getPoolsFromTournament } from "../../../data/pool-functions";
  import { derived } from "svelte/store";
  import ViewToggle from "./_ViewToggle.svelte";

  export let id: string;
  export let poolId: string;

  const hasPoolId = poolId !== undefined;

  var tournamentPromise = findTournamentById(+id);
  var pools$ = getPoolsFromTournament(+id);

  const poolRoutes$ = derived(pools$, (pools) => {
    return pools.map((x) => {
      return {
        id: x.id,
        name: `${x.name}:${x.players.length}`,
      };
    });
  });

  const activePool$ = derived(pools$, (pools) => {
    return pools.find((x) => x.id === +poolId);
  });
</script>

<style>
  .container {
    display: grid;
    grid-template-columns: 49px 1fr;
    border-top: 1px solid var(--nttb-blue);
  }
  .container > .left {
    border-right: 1px solid var(--nttb-blue);
  }
  .pool-nav {
    display: block;
    height: 48px;
    width: 48px;
    line-height: 48px;
    text-align: center;
    font-weight: bold;
    color: black;
    background-color: rgb(244, 244, 244);
    border-bottom: 1px solid var(--nttb-orange);
  }

  .right__header {
    padding-left: 12px;
    font-size: larger;
    font-weight: bold;
    background-color: var(--nttb-orange);
    color: white;
    line-height: 48px;
    height: 48px;
    border-bottom: 1px solid var(--nttb-orange);
  }
  .right__content {
    padding: 8px;
  }
</style>

{#await tournamentPromise}
  <TournamentHeader />
  <ViewToggle mode="ranking" {poolId} {id} />
  <div class="container">
    <p>Loading...</p>
  </div>
{:then tournament}
  <TournamentHeader title={tournament.name} />
  <ViewToggle mode="ranking" {poolId} {id} />

  <div class="container">
    <div class="left">
      {#each $poolRoutes$ as poolRoute}
        <a
          class="pool-nav"
          href="/#/tournament/{id}/play/{poolRoute.id}">{poolRoute.name}</a>
      {/each}
    </div>
    <div class="right">
      {#if hasPoolId}
        <div class="right__header">
          Poule
          {$activePool$.name}
          - MK{$activePool$.players.length}
        </div>
        <div class="right__content">
          <PoolRanking pool={$activePool$} />
        </div>
      {:else}
        {#each $pools$ as pool}
          <h2>Poule {pool.name}</h2>
          <PoolRanking {pool} />
        {/each}
      {/if}
    </div>
  </div>
{/await}