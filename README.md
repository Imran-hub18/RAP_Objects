**Custom Entity

@EndUserText.label: 'Custom Entity For MSFA Asset Change'
define root custom entity ZC_MSFA_ASSET_CHANGE

{
  key CompanyCode   : bukrs;
  key AssetMainNo   : bf_anln1;
  key AssetSubNo    : bf_anln2;
      InventoryDate : abap.dats;
      InventoryNote : abap.char( 15 );
      Plant         : werks_d;
      Message     : bapi_msg;

}

** Behvior definition
unmanaged implementation in class zbp_c_msfa_asset_change unique;
//strict ( 2 ); //Uncomment this line in order to enable strict mode 2. The strict mode has two variants (strict(1), strict(2)) and is prerequisite to be future proof regarding syntax and to be able to release your BO.

define behavior for ZC_MSFA_ASSET_CHANGE //alias <alias_name>
//late numbering
lock master
authorization master ( instance )
//etag master <field_name>
{
  create;
//  field ( readonly : update ) Message;
  update;
  delete;


  mapping for zc_msfa_asset_change
    {
      CompanyCode   = CompanyCode;
      AssetMainNo   = AssetMainNo;
      AssetSubNo    = AssetSubNo;
      InventoryDate = InventoryDate;
      InventoryNote = InventoryNote;
      Plant         = Plant;
      Message       = Message;
    }
}


**Behavior Implementation
CLASS lcl_buffer DEFINITION.
  PUBLIC SECTION.
    CLASS-DATA new_entries TYPE TABLE FOR CREATE zc_msfa_asset_change.
ENDCLASS.


CLASS lhc_ZC_MSFA_ASSET_CHANGE DEFINITION INHERITING FROM cl_abap_behavior_handler.
*  PUBLIC SECTION.!
*    CLASS-DATA Go_Instance   TYPE REF TO lhc_ZC_MSFA_ASSET_CHANGE.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR zc_msfa_asset_change RESULT result.

    METHODS create FOR MODIFY
      IMPORTING entities FOR CREATE zc_msfa_asset_change.

    METHODS update FOR MODIFY
      IMPORTING entities FOR UPDATE zc_msfa_asset_change.

    METHODS delete FOR MODIFY
      IMPORTING keys FOR DELETE zc_msfa_asset_change.

    METHODS read FOR READ
      IMPORTING keys FOR READ zc_msfa_asset_change RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK zc_msfa_asset_change.

ENDCLASS.

CLASS lhc_ZC_MSFA_ASSET_CHANGE IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD create.


    INSERT LINES OF entities INTO TABLE lcl_buffer=>new_entries.

****    DATA : return             TYPE bapiret2,
****           Inventory          TYPE zwif_fi_fixedasset_change=>bapi1022_feglg011,
****           Inventoryx         TYPE zwif_fi_fixedasset_change=>bapi1022_feglg011x,
****           timedependentdata  TYPE zwif_fi_fixedasset_change=>bapi1022_feglg003,
****           timedependentdatax TYPE zwif_fi_fixedasset_change=>bapi1022_feglg003x,
****           message            TYPE bapiret2-message.
****
****
****  READ ENTITIES OF zc_msfa_asset_change IN LOCAL MODE
****  ENTITY zc_msfa_asset_change
****  ALL FIELDS WITH CORRESPONDING #( entities )
****  RESULT DATA(lt_result)
****  REPORTED REPORTED
****  FAILED FAILED.
****
****    DATA(myclass)      = zwfcl_fi_fixedasset_change=>create_instance( ).
****    LOOP AT entities ASSIGNING FIELD-SYMBOL(<fs_entities>).
****      inventory-date = <fs_entities>-InventoryDate.
****      inventoryx-date = 'X'.
****      inventory-note = <fs_entities>-InventoryNote.
****      inventoryx-note = 'X'.
****      timedependentdata-plant = <fs_entities>-Plant.
****      timedependentdatax-plant = 'X'.
****
****
****
****
****
****      TRY.
****          myclass->bapi_fixedasset_change(
****            EXPORTING
*****        allocations           = allocations
*****        allocationsx          = allocationsx
****          asset                 = <fs_entities>-AssetMainNo
****          companycode           = <fs_entities>-CompanyCode
*****        generaldata           = generaldata
*****        generaldatax          = generaldatax
*****        glo_in_gen            = glo_in_gen
*****        glo_in_genx           = glo_in_genx
*****        glo_jp_ann16          = glo_jp_ann16
*****        glo_jp_ann16x         = glo_jp_ann16x
*****        glo_jp_imptd          = glo_jp_imptd
*****        glo_jp_imptdx         = glo_jp_imptdx
*****        glo_jp_ptx            = glo_jp_ptx
*****        glo_jp_ptxx           = glo_jp_ptxx
*****        glo_kr_bus_place      = glo_kr_bus_place
*****        glo_kr_bus_placex     = glo_kr_bus_placex
*****        glo_natl_clfn_code    = glo_natl_clfn_code
*****        glo_natl_clfn_codex   = glo_natl_clfn_codex
*****        glo_pt_fscl_maps      = glo_pt_fscl_maps
*****        glo_pt_fscl_mapsx     = glo_pt_fscl_mapsx
*****        glo_rus_gen           = glo_rus_gen
*****        glo_rus_gentd         = glo_rus_gentd
*****        glo_rus_gentdx        = glo_rus_gentdx
*****        glo_rus_genx          = glo_rus_genx
*****        glo_rus_ltx           = glo_rus_ltx
*****        glo_rus_ltxtd         = glo_rus_ltxtd
*****        glo_rus_ltxtdx        = glo_rus_ltxtdx
*****        glo_rus_ltxx          = glo_rus_ltxx
*****        glo_rus_ptx           = glo_rus_ptx
*****        glo_rus_ptxtd         = glo_rus_ptxtd
*****        glo_rus_ptxtdx        = glo_rus_ptxtdx
*****        glo_rus_ptxx          = glo_rus_ptxx
******        glo_rus_trc           = glo_rus_trc
*****        glo_rus_trcx          = glo_rus_trcx
*****        glo_rus_ttx           = glo_rus_ttx
*****        glo_rus_ttxtd         = glo_rus_ttxtd
*****        glo_rus_ttxtdx        = glo_rus_ttxtdx
*****        glo_rus_ttxx          = glo_rus_ttxx
*****        glo_time_dep          = glo_time_dep
*****        groupasset            = groupasset
*****        insurance             = insurance
*****        insurancex            = insurancex
****          inventory             = inventory
****          inventoryx            = inventoryx
*****        investacctassignmnt   = investacctassignmnt
*****        investacctassignmntx  = investacctassignmntx
*****        leasing               = leasing
*****        leasingx              = leasingx
*****        networthvaluation     = networthvaluation
*****        networthvaluationx    = networthvaluationx
*****        origin                = origin
*****        originx               = originx
*****        postinginformation    = postinginformation
*****        postinginformationx   = postinginformationx
*****        realestate            = realestate
*****        realestatex           = realestatex
****          subnumber             = <fs_entities>-AssetSubNo
****          timedependentdata     = timedependentdata
****          timedependentdatax    = timedependentdatax
****        IMPORTING
****          return                = return
*****      tables
*****        depreciationareas     = depreciationareas
*****        depreciationareasx    = depreciationareasx
*****        extensionin           = extensionin
*****        investment_support    = investment_support
****  ).
****
****
*****       INSERT VALUE #( %key = new-%key
*****                      %msg = new_message( id       = 'ZBS_DEMO_RAP_PATTERN'
*****                                          number   = '011'
*****                                          severity = if_abap_behv_message=>severity-error
*****                                          v1       = new-LocationId ) )
*****             INTO TABLE reported-.
****
*****    APPEND VALUE #(
*****            %cid = <fs_entities>-%cid
*****            %msg = new_message(
*****                     id       = 'ZBAPI'
*****                     number   = return-number
*****                     severity = if_abap_behv_message=>severity-success
*****                   )
*****          ) TO reported-zc_msfa_asset_change.
*****
*****        " Optionally update Message field in entity
*****        MODIFY ENTITIES OF zc_msfa_asset_change IN LOCAL MODE
*****          ENTITY zc_msfa_asset_change
*****          UPDATE FIELDS ( Message )
*****          WITH VALUE #(
*****            (
******             %tky    = <fs_entities>-%cid
*****              Message = return-message )
*****          )
*****          REPORTED reported
*****          FAILED   failed.
****
*****          <fs_entities>-message =   return-message.
*****          APPEND VALUE #(
*****   %msg = new_message_with_text(
*****             severity = if_abap_behv_message=>severity-success
*****             text     = return-message )
****** ) TO reported-zc_msfa_asset_change.
*****
******
*****          MODIFY ENTITIES OF zc_msfa_asset_change IN LOCAL MODE
*****             ENTITY zc_msfa_asset_change
*****             UPDATE FIELDS ( Message  )
*****             WITH VALUE #(  FOR value IN entities (
*****                               %key = value-%key
*****                              Message = return-message
*****                                ) )
*****              REPORTED reported
*****              FAILED  failed.
****
****
****
*****          MODIFY ENTITIES OF zc_msfa_asset_change IN LOCAL MODE
*****               ENTITY zc_msfa_asset_change
*****               UPDATE FROM VALUE #(
*****                 ( %key-CompanyCode          = <fs_entities>-CompanyCode
*****                   Message         = return-Message
*****                   %control-Message = if_abap_behv=>mk-on ) ).
****
****
****
****        CATCH cx_aco_application_exception.
****        CATCH cx_aco_communication_failure.
****        CATCH cx_aco_system_failure.
****          "handle exception
****      ENDTRY.
****    ENDLOOP.
  ENDMETHOD.

  METHOD update.
  ENDMETHOD.

  METHOD delete.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.

ENDCLASS.

CLASS lsc_ZC_MSFA_ASSET_CHANGE DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_ZC_MSFA_ASSET_CHANGE IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.


  ENDMETHOD.

  METHOD save.
*    DATA(lo_data_handler) = zbp_c_msfa_asset_change=>create_instance( ).
*

    DATA : return             TYPE bapiret2,
           Inventory          TYPE zwif_fi_fixedasset_change=>bapi1022_feglg011,
           Inventoryx         TYPE zwif_fi_fixedasset_change=>bapi1022_feglg011x,
           timedependentdata  TYPE zwif_fi_fixedasset_change=>bapi1022_feglg003,
           timedependentdatax TYPE zwif_fi_fixedasset_change=>bapi1022_feglg003x,
           message            TYPE bapiret2-message.


    DATA(myclass)      = zwfcl_fi_fixedasset_change=>create_instance( ).
    LOOP AT lcl_buffer=>new_entries ASSIGNING FIELD-SYMBOL(<fs_entities>)..

      inventory-date = <fs_entities>-InventoryDate.
      inventoryx-date = 'X'.
      inventory-note = <fs_entities>-InventoryNote.
      inventoryx-note = 'X'.
      timedependentdata-plant = <fs_entities>-Plant.
      timedependentdatax-plant = 'X'.

      TRY.
          myclass->bapi_fixedasset_change(
            EXPORTING
*        allocations           = allocations
*        allocationsx          = allocationsx
          asset                 = <fs_entities>-AssetMainNo
          companycode           = <fs_entities>-CompanyCode
*        generaldata           = generaldata
*        generaldatax          = generaldatax
*        glo_in_gen            = glo_in_gen
*        glo_in_genx           = glo_in_genx
*        glo_jp_ann16          = glo_jp_ann16
*        glo_jp_ann16x         = glo_jp_ann16x
*        glo_jp_imptd          = glo_jp_imptd
*        glo_jp_imptdx         = glo_jp_imptdx
*        glo_jp_ptx            = glo_jp_ptx
*        glo_jp_ptxx           = glo_jp_ptxx
*        glo_kr_bus_place      = glo_kr_bus_place
*        glo_kr_bus_placex     = glo_kr_bus_placex
*        glo_natl_clfn_code    = glo_natl_clfn_code
*        glo_natl_clfn_codex   = glo_natl_clfn_codex
*        glo_pt_fscl_maps      = glo_pt_fscl_maps
*        glo_pt_fscl_mapsx     = glo_pt_fscl_mapsx
*        glo_rus_gen           = glo_rus_gen
*        glo_rus_gentd         = glo_rus_gentd
*        glo_rus_gentdx        = glo_rus_gentdx
*        glo_rus_genx          = glo_rus_genx
*        glo_rus_ltx           = glo_rus_ltx
*        glo_rus_ltxtd         = glo_rus_ltxtd
*        glo_rus_ltxtdx        = glo_rus_ltxtdx
*        glo_rus_ltxx          = glo_rus_ltxx
*        glo_rus_ptx           = glo_rus_ptx
*        glo_rus_ptxtd         = glo_rus_ptxtd
*        glo_rus_ptxtdx        = glo_rus_ptxtdx
*        glo_rus_ptxx          = glo_rus_ptxx
**        glo_rus_trc           = glo_rus_trc
*        glo_rus_trcx          = glo_rus_trcx
*        glo_rus_ttx           = glo_rus_ttx
*        glo_rus_ttxtd         = glo_rus_ttxtd
*        glo_rus_ttxtdx        = glo_rus_ttxtdx
*        glo_rus_ttxx          = glo_rus_ttxx
*        glo_time_dep          = glo_time_dep
*        groupasset            = groupasset
*        insurance             = insurance
*        insurancex            = insurancex
          inventory             = inventory
          inventoryx            = inventoryx
*        investacctassignmnt   = investacctassignmnt
*        investacctassignmntx  = investacctassignmntx
*        leasing               = leasing
*        leasingx              = leasingx
*        networthvaluation     = networthvaluation
*        networthvaluationx    = networthvaluationx
*        origin                = origin
*        originx               = originx
*        postinginformation    = postinginformation
*        postinginformationx   = postinginformationx
*        realestate            = realestate
*        realestatex           = realestatex
          subnumber             = <fs_entities>-AssetSubNo
          timedependentdata     = timedependentdata
          timedependentdatax    = timedependentdatax
        IMPORTING
          return                = return
*      tables
*        depreciationareas     = depreciationareas
*        depreciationareasx    = depreciationareasx
*        extensionin           = extensionin
*        investment_support    = investment_support
  ).

          <fs_entities>-Message = return-message.

        CATCH cx_aco_application_exception.
        CATCH cx_aco_communication_failure.
        CATCH cx_aco_system_failure.
          "handle exception
      ENDTRY.

**          APPEND VALUE #( %msg = new_message( id       = return-id
**                                              number   = return-number
**                                              v1       = return-message_v1
**                                              v2       = return-message_v2
**                                              v3       = return-message_v3
**                                              v4       = return-message_v4
**                                              severity = if_abap_behv_message=>severity-success )
**                          %key-CompanyCode = <fs_entities>-CompanyCode
**                          %key-AssetMainNo = <fs_entities>-AssetMainNo
**                          %key-AssetSubNo = <fs_entities>-AssetSubNo
***                          %cid =  <fs_entities>-%cid_ref
**                          %update = 'X'
**                          %element-Message = return-message )
**                          TO reported-zc_msfa_asset_change.

      APPEND VALUE #(
        %msg = new_message(
                  id       = return-id
                  number   = return-number
                  severity = if_abap_behv_message=>severity-success )
        %key-CompanyCode = <fs_entities>-CompanyCode
        %key-AssetMainNo = <fs_entities>-AssetMainNo
        %key-AssetSubNo  = <fs_entities>-AssetSubNo
        %element-Message = abap_true
      ) TO reported-zc_msfa_asset_change.

    ENDLOOP.

  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.

**service definition
@EndUserText.label: 'Service Defination for MSFA ASSET CHANGE'
define service ZAPI_MSFA_ASSET_CHANGE {
  expose ZC_MSFA_ASSET_CHANGE;
}

Then create serivce bindig on this .
