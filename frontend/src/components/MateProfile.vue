<template>
<div>
  <div>
    <md-dialog :md-active.sync="showDialog" ref="dialog">
    <md-dialog-title>Update Flatmate Information</md-dialog-title>
    <md-dialog-content>
      <div class="md-layout md-gutter">
        <div class="md-layout-item md-small-size-100">
          <md-field>
            <label for="name">Name</label>
            <md-input name="name" id="name" autocomplete="name" v-model="form.name" :disabled="sending" />
          </md-field>
        </div>
        <div class="md-layout-item md-small-size-100">
          <md-field>  
            <label for="ip">IP Address</label>
            <md-input name="ip" id="ip" v-model="form.ip" :disabled="sending" />
          </md-field>
        </div>
        <div class="md-layout-item md-small-size-100">
          <md-field>
            <label for="gpio">GPIO-Slot</label>
            <md-select name="gpio" id="gpio" v-model="form.gpio" md-dense :disabled="sending">
              <md-option></md-option>
              <md-option value="1">GPIO 1</md-option>
              <md-option value="2">GPIO 2</md-option>
              <md-option value="3">GPIO 3</md-option>
              <md-option value="4">GPIO 4</md-option>
            </md-select>
            <span class="md-error">The GPIO-Slot is required!</span>
          </md-field>
        </div>
      </div>
    </md-dialog-content>
    <md-dialog-actions>
        <md-button @click="showDialog = false">Close</md-button>
        <md-button @click="showDialog = false">Save</md-button>
    </md-dialog-actions>
  </md-dialog>
  </div>
  <div>
    <md-card>
      <md-card-header>
        <div class="md-title"><b>Pinging Address of Flatmates</b></div>
      </md-card-header>
      <md-layout v-for="mate in mates" v-bind:key="mate">
        <md-card class="child-md">
          <md-card-header>
            <div class="md-title">{{mate.name}}</div>
            <md-button class="md-icon-button" @click="openDialog(mate)">
              <md-icon>edit</md-icon>
            </md-button>
          </md-card-header>
          <p><b>IP: </b>{{mate.ip}}</p>
          <p><b>GPIO-Slot: </b>{{mate.gpio_slot}}</p>
        </md-card>
      </md-layout>
    </md-card>
  </div>
</div>
</template>

<script>
export default {
  name: 'MateProfile',
  created: function () {
    const Database = require('better-sqlite3');
    /* eslint-disable no-console */
    const db = new Database('wiah.db', { verbose: console.log });
    const row = db.prepare('SELECT * FROM mates WHERE id=1');
    console.log(row.firstName, row.lastName, row.email);
  },
  data: () => ({
    form: {
        name: null,
        gpio: null,
        ip: null,
      },
    showDialog: false,
    mates: [
      { name: 'Emma', ip: '111.111.111.111', gpio_slot: '1' },
      { name: 'Yannick', ip: '111.111.111.111', gpio_slot: '2' },
      { name: 'Louis', ip: '111.111.111.111', gpio_slot: '3' },
      { name: 'Moritz', ip: '111.111.111.111', gpio_slot: '4' },
    ]
  }),
  methods: {
    openDialog(mateData){
      this.form.name = mateData.name;
      this.form.gpio = mateData.gpio;
      this.form.ip = mateData.ip;

      this.showDialog = true;
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .md-card {
    width: 80vw;
    margin: 4px;
    display: inline-block;
    vertical-align: top;
  }
  .child-md {
    width: 18vw;
  }
  .md-dialog {
    max-width: 768px;
  }
</style>
