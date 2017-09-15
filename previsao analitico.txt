CREATE OR REPLACE VIEW VDIC_PREVISAO_ANALITICO
AS
SELECT con_pag . cd_fornecedor cd_fornecedor,
       con_pag . ds_fornecedor nm_fornecedor,
       con_pag . nr_documento || '/' || to_char(itcon_pag . nr_parcela, '000') nr_documento,
       con_pag . ds_con_pag ds_con_pag,
       trunc(con_pag . dt_emissao) dt_emissao,
       trunc(con_pag . dt_lancamento) dt_lancamento,
       trunc(itcon_pag . dt_vencimento) dt_vencimento,
       trunc(itcon_pag . dt_prevista_pag) dt_prevista_pag,
       con_pag . cd_con_pag cd_con_pag,
       nvl(itcon_pag . vl_duplicata, 0) vl_duplicata,
       itcon_pag.nr_parcela,
       con_pag . cd_previsao cd_previsao,
       NVL(itcon_pag . vl_duplicata, 0) -
       DECODE(null,
              'N',
              0,
              SUM(NVL(pagcon_pag . vl_pago, 0) +
                  (NVL(pagcon_pag . vl_desconto, 0) -
                   NVL(pagcon_pag . vl_acrescimo, 0)))) vl_devido,
       ITCON_PAG . CD_MOEDA CD_MOEDA,
       ITCON_PAG . VL_MOEDA VL_MOEDA,
       case itcon_pag . tp_quitacao  when 'C'  then 'Comprometido'
                                     when 'P'  then 'Parcialmente-Pago'
         else 'Devedor/Previsto'
           end tp_quitacao 
  FROM dbamv . con_pag,
       dbamv . itcon_pag,
       (select *
          from dbamv . pagcon_pag
         where nvl(pagcon_pag . sn_estorno, 'N') = 'N') pagcon_pag
 WHERE con_pag.cd_con_pag = itcon_pag.cd_con_pag
   and itcon_pag.cd_itcon_pag = pagcon_pag.cd_itcon_pag(+)
   and con_pag.cd_con_pag_dev is null
   and nvl(con_pag.vl_bruto_conta, 0) > 0
   and itcon_pag.cd_con_pag_agrup is null
   and nvl(itcon_pag.vl_duplicata, 0) > 0
   AND ((pagcon_pag.dt_baixa IS NULL OR ITCON_PAG.TP_QUITACAO <> 'Q') 
   OR ('01/09/2017' is not NULL  AND pagcon_pag.dt_baixa IS NOT NULL 
   AND ('12/09/2017' < pagcon_pag.dt_baixa OR ITCON_PAG.TP_QUITACAO <> 'Q')))
 GROUP BY con_pag.cd_fornecedor,
          con_pag.ds_fornecedor,
          con_pag.nr_documento || '/' ||
          to_char(itcon_pag.nr_parcela, '000'),
          con_pag.ds_con_pag,   
          trunc(con_pag.dt_emissao),
          trunc(con_pag.dt_lancamento),
          trunc(itcon_pag.dt_vencimento),
          trunc(itcon_pag.dt_prevista_pag),
          con_pag.cd_con_pag,
          con_pag.cd_multi_empresa,
          nvl(itcon_pag.vl_duplicata, 0),
          itcon_pag.nr_parcela,
          con_pag.cd_previsao,
          ITCON_PAG.CD_MOEDA,
          ITCON_PAG.VL_MOEDA,
          itcon_pag.tp_quitacao
 ORDER BY 
          4 ASC,
          6 ASC,
          7 ASC,
          8 ASC,
          9 ASC,
          DS_FORNECEDOR,
          con_pag.nr_documento || '/' ||
          to_char(itcon_pag.nr_parcela, '000')
          
    
