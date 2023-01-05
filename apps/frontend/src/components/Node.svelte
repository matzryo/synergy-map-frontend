<script lang="ts">
  import { onMount } from "svelte";
  import * as Scry from "scryfall-sdk";
  import * as vis from "vis-network/standalone";

  let mynetwork;

  onMount(async () => {
    const chalice = await getCard(67330);

    // ノードを作成
    const nodes = new vis.DataSet([
      {
        id: 1,
        shape: "image",
        image:
          "https://cards.scryfall.io/small/front/1/f/1f0d2e8e-c8f2-4b31-a6ba-6283fc8740d4.jpg?1562433485",
      },
      { id: 2, shape: "image", image: chalice.image_uris.large },
      { id: 3, label: "Node 3" },
      { id: 4, label: "Node 4" },
      { id: 5, label: "Node 5" },
    ]);

    // エッジを作成
    const edges = new vis.DataSet([
      { id: 1, from: 1, to: 3 },
      { id: 2, from: 1, to: 2 },
      { id: 3, from: 2, to: 4 },
      { id: 4, from: 2, to: 5 },
      { id: 5, from: 3, to: 3 },
    ]);

    // ネットワーク全体を作成
    const data = {
      nodes: nodes,
      edges: edges,
    };
    const options = {};
    const network = new vis.Network(mynetwork, data, options);
  });

  const getCard = (arenaId: number) => Scry.Cards.byArenaId(arenaId);
</script>

<div id="mynetwork" bind:this={mynetwork} />

<style type="text/css">
  #mynetwork {
    width: 600px;
    height: 400px;
    border: 1px solid lightgray;
    background-color: white;
  }
</style>
