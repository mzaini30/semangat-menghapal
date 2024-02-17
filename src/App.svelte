<script>
  import Router from "svelte-spa-router";
  import Gerbang from "./kunci/src/Gerbang.svelte";
  import data from "../android.json";
  import Beranda from "./Beranda.svelte";
  import alasql from "alasql";

  alasql(/* sql */ `create localStorage database if not exists data;
  attach localStorage database data;
  use data;
  `);
  // hapalan: id, hapalan
  alasql(
    /* sql */ `CREATE TABLE IF NOT EXISTS hapalan (id text, hapalan INTEGER)`
  );

  // ini insert kalau data kosong aja ya. salah logicnya ini
  let banyakBaris = alasql(/* sql */ `select count(*) from hapalan`)[0].count;
  if (banyakBaris == 0) {
    alasql(
      /* sql */ `INSERT INTO hapalan VALUES ('${crypto.randomUUID()}', 0)`
    );
  }

  if (localStorage.database) {
    let id = alasql(/* sql */ `select id from hapalan `)[0].id;

    let hapalanDatabaseVersiLawas = JSON.parse(localStorage.database).hapalan[0]
      .hapalan;

    alasql(
      /* sql */ `update hapalan set hapalan = ${hapalanDatabaseVersiLawas} where id = '${id}' `
    );
  }

  const routes = {
    "/": Beranda,
  };
</script>

<Gerbang namaAplikasi={data.title}>
  <Router {routes} />
</Gerbang>
