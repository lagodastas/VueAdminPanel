<template>
  <div>

    <div id="customForm">
      <fieldset class="name">
        <legend data-i18n="Name">Name</legend>
        <editor-field name="colors.name" v-pre></editor-field>
        <editor-field name="colors.state" v-pre></editor-field>
      </fieldset>
    </div>

    <h2>Implement jQuery DataTable in Vue Js</h2>
    <table class="table display responsive" id="colorsTable">
      <thead>
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>State</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in products" :key="item.colors.id">
        <td>{{ item.colors.id }}</td>
        <td>{{ item.colors.name }}</td>
        <td>{{ item.colors.state }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import "jquery/dist/jquery.min.js";
import "../../assets/css/custom-colors.css";


import "datatables.net/js/jquery.dataTables.min.js";
import "datatables.net-dt/js/dataTables.dataTables.min.js";
import "datatables.net-responsive/js/dataTables.responsive.min.js?v=3";
import "datatables.net-responsive-dt/js/responsive.dataTables.min.js";


//import "datatables.net-dt/js/select/editor.select2.js";

import "datatables.net-dt/js/select/select2.min.js";

//import "datatables.net-dt/js/select/it.js";


import "DataTables/js/dataTables.editor.js?v=1";


import "datatables.net-dt/js/select/dataTables.select.min.js";

import "datatables.net-dt/js/select/dataTables.bootstrap4.min.js";


import "DataTables/js/dataTables.buttons.js";
//import "datatables.net-dt/js/buttons/buttons.html5.min.js";
//import "datatables.net-dt/js/buttons/buttons.print.min.js";


import "datatables.net-dt/js/checkboxes/dataTables.checkboxes.min.js";
import "datatables.net-dt/js/rowgroup/dataTables.rowGroup.min.js";



import "datatables.net-dt/css/jquery.dataTables.min.css";
import "datatables.net-responsive-dt/css/responsive.dataTables.min.css";
import "datatables.net-dt/css/buttons/buttons.dataTables.min.css";
import "datatables.net-dt/css/select/select.dataTables.min.css";
//import "select2/css/select2.min.css";
import "datatables.net-dt/css/select/select2.min.css";
import "datatables.net-dt/css/select/select2-bootstrap.min.css?v=2";
import "datatables.net-dt/css/select/awesome-bootstrap-checkbox.css";
import "datatables.net-dt/css/select/dataTables.bootstrap4.min.css";
import "datatables.net-dt/css/select/dataTables.checkboxes.css";
import "datatables.net-dt/css/select/rowGroup.bootstrap4.min.css";
import "DataTables/css/editor.dataTables.css";


import $ from "jquery";
export default {
  mounted() {
    let editor;

    editor = new $.fn.dataTable.Editor( {
          ajax: {
            url: "http://prosahin.loc/api/colors.php",
            type: "POST"
          },
          template: '#customForm',
          table: "#colorsTable",
          "theme": "bootstrap",
          idSrc:  'colors.id',
          fields: [
              {
                label: "ID:",
                name: "colors.id"
              },
              {
                label: "<span data-i18n='Name' >Name</span>",
                name: "colors.name"
              },
              {
                label: "<span data-i18n='Groups' >Groups</span>",
                name: "colors.group_id",
                "type": "select",
              },
              {
                "label": "<span data-i18n='State' >State</span>",
                "name": "colors.state",
                "type": "select",
                "options": [
                  {value: "new", label: "New"},
                  {value: "checked", label: "Checked"}
                ],
                "opts": {
                    "theme": "bootstrap",
                    "language": "en",
                    "placeholder": "State",
                    "allowClear": true,
                },
              }
            ],
              formOptions: {
                main: {
                  scope: 'cell' // Allow multi-row editing with cell selection
                }
              }
            } );

            editor.on( 'open', function ( e, type ) {
                editor.buttons( [
                  {
                    text: '<span data-i18n="Cancel">Cancel</span>',
                                "label": "Cancel",
                                "className":"mg-x-5",
                                "fn": function () {
                editor.close();
              }
              },
              {
                text: '<span data-i18n="Edit">Edit</span>',
                                "label": "Save",
                                "className":"mg-x-5",
                                "fn": function () {
            editor.submit();
          }
          }
        ]);

        //updateText();
        } );


        // Activate an inline edit on click of a table cell
        $('#colorsTable').on( 'dblclick', 'tbody td:not(:first-child)', function (e) {
          //editor.inline( this );
        } );


        editor.on( 'preSubmit', function ( e, o, action ) {
            var is_in_use = 0;
            if(editor.mode() == 'remove'){
              $.ajax({
                url: 'http://prosahin.loc/api/colors.php',
                dataType: 'json',
                async: false,
                data: {
                  'is_color_in_use':'yes',
                  'color_id':table.rows( {selected: true} ).data()[0].colors.id
                },
                success: function(d) {
                  is_in_use = d.is_in_use;
                }
              });
            }

            if(is_in_use > 0){
              alert("Color is used");
              return false;
            }

        } );


    let table = $("#colorsTable").DataTable(
        {
          processing: true,
          responsive: true,
          serverSide: true,
          stateSave: false,
          "lengthMenu": [[10, 25, 50, 300, 600, -1], [10, 25, 50, 300, 600, "All"]],
          pageLength: 50,
          pagingType: "numbers",
          language: {
            url: "/lib/DataTables/lang/" + $('.language option:selected').val() + ".json"
          },
          paging: true,
          ordering: true,
          order: [[1, 'DESC']],
          "autoWidth": false,
          dom: 'Blfrtip',
          ajax: {
            url: "http://prosahin.loc/api/colors.php",
            type: "POST",
            data: function (d) {
              d.show_all = $('#colorsTable_wrapper .show_all').prop('checked');
            },
          },
          display: 'bootstrap',
          columns: [
            { data: 'colors.id', className: 'wd-5p'},
            { data: "colors.name", className: 'editable' },
            { data: "colors.state", className: 'editable' },
          ],
          select: true,


          buttons: [
            {
              text: '<span data-i18n="Reload">Reload</span>',
              action: function ( e, dt, node, config ) {
                dt.ajax.reload();
              },
            },
            { extend: "create", text: '<span data-i18n="Create">Create</span>', editor: editor },
            {
              text: '<span data-i18n="Edit">Edit</span>',
              className: 'custom_edit_btn',
              editor: editor,
              action: function ( e, dt, node, config ) {
                editor
                  .edit( table.rows( {selected: true} ).indexes(), {
                    title: '<span data-i18n="Edit">Edit</span>',
                    buttons: '<span data-i18n="Edit">Edit</span>',
                    className: 'custom_edit_btn'
                  } )
                  .mode( 'edit' );
              }
            },
            {
              text: '<span data-i18n="Remove">Remove</span>',
              className: 'custom_remove_btn',
              editor: editor,
              action: function ( e, dt, node, config ) {
                editor
                  .remove( table.rows( {selected: true} ).indexes(), {
                    title: '<span data-i18n="Remove">Remove</span>',
                    buttons: '<span data-i18n="Remove">Remove</span>',
                    className: 'custom_edit_btn'
                  } )
                  .mode( 'remove' );
              }
            },
            {
              extend: "selected",
              className: 'custom_copy_btn',
                text: '<span data-i18n="Duplicate">Duplicate</span>',
                action: function ( e, dt, node, config ) {
                // Start in edit mode, and then change to create
                editor
                  .edit( table.rows( {selected: true} ).indexes(), {
                    text: '<span data-i18n="Duplicate">Duplicate</span>',
                    buttons: 'Create from existing'
                  } )
                  .mode( 'create' );
              }
            },
            {
              extend: 'collection',
              text: 'Export',
              buttons: [
                'copy',
                'excel',
                'csv',
                'pdf',
                //'print'
              ]
            },
            {
              extend: 'excel',
              title: null,
              text: '<span data-i18n="Excel">Excel</span>',
              className: 'custom_excel_btn',
              exportOptions: {modifier: {selected: null}}
            }
          ]
        }
      );
  },
  data: function () {
    return {
      products: [],
    };
  },
};
</script>
