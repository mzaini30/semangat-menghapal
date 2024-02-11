<script>
  import { acak } from "kumpulan-tools";
  import Judul from "./komponen/Judul.svelte";
  import Kartu from "./komponen/Kartu.svelte";
  import penyemangat from "./data/penyemangat.json";
  import { Orm } from "./orm";
  import { Button, Modal, Label, Input } from "flowbite-svelte";
  import Edit from "./icon/Edit.svelte";

  let clickOutsideModal = false;

  let motivasi = acak(penyemangat.motivational_quotes)[0].quote;

  let database = {};

  function simpan() {
    localStorage.database = JSON.stringify(database);
  }

  if (localStorage.database) {
    database = JSON.parse(localStorage.database);
  }

  if (!database.hapalan) {
    database = new Orm(database)
      .insert("hapalan", {
        hapalan: 0,
      })

      .done();
    localStorage.idPertama = database.hapalan[0].id;
    simpan();
  }
</script>

<div class="p-5">
  <Kartu>
    <Judul teks="Nomor halaman yang terakhir dihapal"></Judul>
    <p class="text-center text-[50px]">{database.hapalan[0].hapalan}</p>
  </Kartu>
  <Kartu>
    <Judul className="mb-3" teks="Catatan Penyemangat"></Judul>
    <p>{motivasi}</p>
  </Kartu>
</div>
<Button
  on:click={() => (clickOutsideModal = true)}
  pill={true}
  class="!p-2 fixed bottom-3 right-3"><Edit></Edit></Button
>
<Modal title="Update Pencapaian" bind:open={clickOutsideModal}>
  <div class="mb-6">
    <Label for="default-input" class="block mb-2"
      >Pencapaian baru (nomor halaman)</Label
    >
    <Input
      id="default-input"
      type="tel"
      bind:value={database.hapalan[0].hapalan}
    />
    <div class="mt-3 grid grid-cols-5 gap-3">
      {#each Array(10) as x, n}
        <Button
          on:click={() => {
            database.hapalan[0].hapalan = +n + +database.hapalan[0].hapalan + 1;
          }}>{+n + +database.hapalan[0].hapalan + 1}</Button
        >
      {/each}
    </div>
  </div>
  <svelte:fragment slot="footer">
    <Button
      on:click={() => {
        simpan();
        clickOutsideModal = false;
      }}>Simpan</Button
    >
  </svelte:fragment>
</Modal>
