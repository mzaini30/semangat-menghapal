<script>
  import Router from "svelte-spa-router";
  import Gerbang from "./kunci/src/Gerbang.svelte";
  import data from "../android.json";
  import Beranda from "./Beranda.svelte";

  const routes = {
    '/': Beranda
  };
</script>

<Gerbang namaAplikasi={data.title}>
  <Router {routes} />
</Gerbang>
