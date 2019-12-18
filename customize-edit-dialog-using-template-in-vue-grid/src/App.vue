<template>
  <div id="app">
    <ejs-grid :dataSource="data" :editSettings="editOptions" :toolbar="toolbarOptions">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" :isPrimaryKey="true"></e-column>
        <e-column field="CustomerID" headerText="Customer ID"></e-column>
        <e-column field="ShipName" headerText="ShipName"></e-column>
        <e-column field="ShipCountry" headerText="Ship Country"></e-column>
      </e-columns>
    </ejs-grid>
  </div>
</template>

<script>
import Vue from "vue";
import { data } from "./dataSource.js";
import { GridPlugin, Edit, Toolbar } from "@syncfusion/ej2-vue-grids";
import { DropDownListPlugin } from "@syncfusion/ej2-vue-dropdowns";
import { NumericTextBoxPlugin } from "@syncfusion/ej2-vue-inputs";
import { DatePickerPlugin } from "@syncfusion/ej2-vue-calendars";
import { DataUtil } from "@syncfusion/ej2-data";
Vue.use(GridPlugin);
Vue.use(DropDownListPlugin);
Vue.use(NumericTextBoxPlugin);
Vue.use(DatePickerPlugin);

export default {
  name: "app",
  data() {
    return {
      data: data,
      editOptions: {
        allowAdding: true,
        allowEditing: true,
        allowDeleting: true,
        mode: "Dialog",
        template: function() {
          return {
            template: Vue.component("DailogTemplate", {
              template: `<div formGroup="orderForm">
                <div class="form-row">
                  <div class="form-group col-md-6">
                    <div class="e-float-input e-control-wrapper">
                      <input id="OrderID" name="OrderID" v-model="data.OrderID" type="text" :disabled="!data.isAdd">
                      <label class="e-float-text e-label-top" for="OrderID"> Order ID </label>
                    </div>
                  </div>
                  <div class="form-group col-md-6">
                    <ejs-dropdownlist id="CustomerID" name="CustomerID" v-model="data.CustomerID" placeholder="Customer Name"
                    :dataSource="CustomerDistinctData" :fields="{text: 'CustomerID', value: 'CustomerID' }" 
                    popupHeight='300px' floatLabelType='Always'></ejs-dropdownlist>
                  </div>
                </div>
                <div class="form-row">
                  <div class="form-group col-md-6">
                    <ejs-numerictextbox id="Freight" name="Freight" v-model="data.Freight" placeholder="Freight" floatLabelType='Always'>
                    </ejs-numerictextbox>
                  </div>
                  <div class="form-group col-md-6">
                    <ejs-datepicker id="OrderDate" name="OrderDate" v-model="data.OrderDate" placeholder="Order Date" 
                    floatLabelType='Always'></ejs-datepicker>
                  </div>
                </div>
                <div class="form-row">
                  <div class="form-group col-md-12">
                    <div class="e-float-input e-control-wrapper">
                      <textarea id="ShipAddress" name="ShipAddress" v-model="data.ShipAddress" type="text"></textarea>
                      <label class="e-float-text e-label-top" for="ShipAddress"> Ship Address </label>
                    </div>
                  </div>
                </div>
              </div>`,
              data() {
                return {
                  data: {}
                };
              },
              computed: {
                CustomerDistinctData: () => {
                  return DataUtil.distinct(data, "CustomerID", true);
                }
              }
            })
          };
        }
      },
      toolbarOptions: ["Add", "Edit", "Delete"]
    };
  },
  provide: {
    grid: [Edit, Toolbar]
  }
};
</script>

<style>
@import url("https://cdn.syncfusion.com/ej2/material.css");
@import "https://stackpath.bootstrapcdn.com/bootstrap/4.1.2/css/bootstrap.min.css";
</style>
