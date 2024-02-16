<script>
  import Router from "svelte-spa-router";
  import Gerbang from "./kunci/src/Gerbang.svelte";
  import data from "../android.json";
  import Beranda from "./Beranda.svelte";
  import alasql from "alasql";

  alasql(/* sql */ `create localStorage database if not exists data;
  attach localStorage database data`);
  alasql(
    /* sql */ `CREATE TABLE IF NOT EXISTS data.hapalan (id text, hapalan INTEGER)`
  );

  // ini insert kalau data kosong aja ya. salah logicnya ini
  alasql(
    /* sql */ `INSERT INTO data.hapalan VALUES ('${crypto.randomUUID()}', 0)`
  );

  if (localStorage.database) {
    let id = alasql(/* sql */ `select id from data.hapalan `)[0].id;

    let hapalanDatabaseVersiLawas = JSON.parse(localStorage.database).hapalan[0]
      .hapalan;

    alasql(
      /* sql */ `update data.hapalan set hapalan = ${hapalanDatabaseVersiLawas} where id = '${id}' `
    );
  }

  const routes = {
    "/": Beranda,
  };
</script>

<Gerbang namaAplikasi={data.title}>
  <Router {routes} />
</Gerbang>
