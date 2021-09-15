# NDRHD
 @@ -0,0 +1,34 @@
 @AbapCatalog.sqlViewName: 'ZCDS_SEM_DES_MS'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Semaforo de existencias'
@OData.publish: true
define view ZCDS_SEMAF_DESPL
  with parameters
    P_cen : werks_d,
    P_mat : matnr
  as select from zcds_exist_ms as a
{
  key
  a.centro          as ApiCen,
  key
  a.material        as ApiMat,
  @Semantics.amount.currencyCode: 'Disponible'
  a.stockdisponible as ApiDisp,
  @Semantics.text: true
  case
   when a.stockdisponible > 0  and a.diasinv = 0
   then 3
   when a.diasinv > 3
   then 2
   when a.diasinv >= 1 and a.diasinv <= 3
   then 1
  else  0
  end               as ApiDespl
}
where        
      a.centro      = $parameters.P_cen
  and a.material    = $parameters.P_mat
  and a.stsmaterial = 'SB'  
 
