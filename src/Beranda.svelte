<script>
  import { acak } from "kumpulan-tools";
  import Judul from "./komponen/Judul.svelte";
  import Kartu from "./komponen/Kartu.svelte";
  import penyemangat from "./data/penyemangat.json";
  import { Orm } from "./orm";
  import { Button, Modal, Label, Input } from "flowbite-svelte";
  import Edit from "./icon/Edit.svelte";

  let clickOutsideModal = true;

  let motivasi = acak(penyemangat.motivational_quotes)[0].quote;

  let database = {};

  function simpan() {
    localStorage.database = JSON.stringify(database);
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

  if (localStorage.database) {
    database = JSON.parse(localStorage.database);
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
<Modal
  title="Update Pencapaian"
  bind:open={clickOutsideModal}
  autoclose
  outsideclose
>
  <div class="mb-6">
    <Label for="default-input" class="block mb-2"
      >Pencapaian baru (nomor halaman)</Label
    >
    <Input id="default-input" type="tel" />
  </div>
</Modal>
