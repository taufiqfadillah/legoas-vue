<template>
    <div>
      <div class="col s12">
        <div class="card">
          <div class="card-content">
            <div class="row no-margin">
              <div id="viewAll" class="col s12">
                <div class="row no-margin">
                  <div class="col s12">
                    <Table
                      :offLineMode="true"
                      :fields="tableComponentData.fields"
                      :loadOfflineEntries="loadOfflineEntries"
                       defaultSort="id"
                       defaultSortDir="asc"
                       tableUniqueId="table_piutang"
                      :perPageOptions="tableComponentData.perPageOptions"
                      :hideSearchBox="false"
                      tableTitle="Piutang Unit"
                      actions="true"
                      :actionList="tableComponentData.actionList"
                      :defaultSearchColumns="['id', 'name']"
                      :showSearchFilter="true"
                      searchByFilterName="This is Filter Title"
                      :showSearchFilterNotification="true"
                      :showColumnFilter="true"
                      columnFilterName="This is column show/hide options"
                      :showColumnFilterNotification="true"
                      :exportExcel="true"
                      :moveableColumn="true"
                      :showColumnFilterButton="false"
                      :showLockColumnsButton="false"
                      :memorizeSettingsLocally="false"
                      :addAllOptionInPagination="true"
                      >
                    </Table>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import Table from "data-table-vue-v3"; 
import $ from "jquery";
import 'materialize-css/dist/js/materialize.min.js'

export default {
 components: { Table , $}, 
 data() {
   return {
     tableComponentData: {
        perPageOptions: [
        {
          displayText: 10,
          value: 10,
          defaultSelected: true
        }, {
          displayText: 20,
          value: 20
        }, {
          displayText: 50,
          value: 50
        }, {
          displayText: 100,
          value: 100
        }],
       fields: [
         {
           name: "ID",
           key: "id",
           lockable: true,
           lock: true,
           sortable: true
         },
         {
           name: "No. Kewajiban",
           key: "no_kewajiban",
           lockable: true,
           lock: false,
           sortable: true
         },
         {
           name: "No. Polisi",
           key: "no_polisi",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Pemilik",
           key: "pemilik",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Peserta",
           key: "peserta",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Nomor VA",
           key: "no_va",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Harga Terbentuk (Rp)",
           key: "harga_terbentuk",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Biaya Admin ex PPN (Rp)",
           key: "biaya_admin",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "PPN (Rp)",
           key: "ppn",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Total (Rp)",
           key: "total",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Tanggal Lelang",
           key: "tanggal_lelang",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Tanggal Jatuh Tempo",
           key: "tanggal_jatuh_tempo",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Tanggal Lunas",
           key: "tanggal_lunas",
           multiFilterOption: true,
           sortable: true
         },
         {
           name: "Status",
           key: "status",
           multiFilterOption: true,
           sortable: true,
         },
       ],
        actionList: [
         {
           type: "cb",
           refAddress: data => this.changeStatus(data.id),
           getIcon: data => {
            return data.status === 'Proses Pembayaran' ? 'not_interested' : data.status === 'Lunas' ? 'offline_pin' : 'pause_circle_filled';
            },
           hoverTitle: "Click to change status",
         }
       ]
     }
   };
 },
 methods: {
  getData() {
    if (localStorage.getItem('randomData')) {
      return JSON.parse(localStorage.getItem('randomData'));
    }

    let randomData = []
    let names = ['Sutisna', 'Joko', 'Budi', 'Agus', 'Rina', 'Sari', 'Dewi', 'Asep', 'Eko', 'Rudi', 'Abdel', 'Firman', 'Adi', 'Agung', 'Sultan', 'Bowo', 'Denis'];

    for (let i = 0; i < 25; i++) {
      let date = new Date();
      date.setDate(date.getDate() + i);
      let formattedDate = date.toLocaleDateString('id-ID');

      let formatRp = new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
        minimumFractionDigits: 0,
      });

    let values = [null, true, false];
    let statusValue = values[Math.floor(Math.random() * values.length)];

    let status;
        if (statusValue === null) {
            status = 'Proses Pembayaran';
        } else if (statusValue === true) {
            status = 'Lunas';
        } else {
            status = 'Konfirmasi Pembayaran';
        }

      randomData.push({
        id: i + 1,
        uniqueKey: 'unique_' + (i + 1),
        no_kewajiban: Math.floor(Math.random() * 1000000000),
        no_polisi: Math.floor(Math.random() * 1000000000),
        pemilik: names[Math.floor(Math.random() * names.length)],
        peserta: names[Math.floor(Math.random() * names.length)],
        no_va: Math.floor(Math.random() * 1000000000),
        harga_terbentuk: formatRp.format(Math.floor(Math.random() * 1000000)),
        biaya_admin: formatRp.format(Math.floor(Math.random() * 1000000)),
        ppn: formatRp.format(Math.floor(Math.random() * 1000000)),
        total: formatRp.format(Math.floor(Math.random() * 1000000)),
        tanggal_lelang: formattedDate,
        tanggal_jatuh_tempo: formattedDate,
        tanggal_lunas: formattedDate,
        status: status,
      });
    }
    localStorage.setItem('randomData', JSON.stringify(randomData));

    return randomData;
  },
  changeStatus(id) {
    let data = this.getData();
    let index = data.findIndex(item => item.id === id);
    if (index !== -1) {
      if (data[index].status === 'Proses Pembayaran') {
        data[index].status = 'Konfirmasi Pembayaran';
      } else if (data[index].status === 'Konfirmasi Pembayaran') {
        data[index].status = 'Lunas';
      } else if (data[index].status === 'Lunas') {
        data[index].status = 'Proses Pembayaran';
      }
      localStorage.setItem('randomData', JSON.stringify(data));
      this.tableComponentData = this.getData();
      location.reload();
    }
  },
  loadOfflineEntries(cb) {
    let data = {
      status: 200,
      msg: this.getData()
    };
    cb(data);
  }
}
};
</script>

<style>
</style>