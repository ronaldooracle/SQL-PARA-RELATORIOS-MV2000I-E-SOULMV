select ratcon_pag . cd_con_pag,
       ratcon_pag.cd_reduzido,
       PLANO_CONTAS . CD_CONTABIL CD_CONTABIL,
       PLANO_CONTAS . DS_CONTA DS_CONTABIL,
       dbamv . con_pag.ds_con_pag,
       ratcon_pag . cd_item_res,
       item_res . ds_item_res,
       con_pag . dt_lancamento,
       nvl(setor . nm_setor, 'NÃO INFORMADO') nm_setor,
       con_pag . ds_fornecedor,
       nvl(ratcon_pag . nr_meses_amortizacao, 0) nr_meses_amortizacao,
       nvl(ratcon_pag . vl_rateio, 0) vl_rateio,
       decode(tp_item_res, 'C', 'Custo', 'R', 'Resultado', 'Ambos') ds_tp_item_res,
       ratcon_pag . sn_importado_custos,
       con_pag . nr_documento,
       ratcon_pag . dt_competencia
  from dbamv . ratcon_pag,
       dbamv . item_res,
       dbamv . setor,
       dbamv . con_pag,
       DBAMV . PLANO_CONTAS
 where ratcon_pag.cd_item_res is not null
   and item_res.cd_item_res = ratcon_pag.cd_item_res
   and PLANO_CONTAS.CD_REDUZIDO = ratcon_pag.cd_reduzido
   and setor.cd_setor(+) = ratcon_pag.cd_setor
   and con_pag.cd_con_pag(+) = ratcon_pag.cd_con_pag
   and ratcon_pag . dt_competencia between '01/09/2016' and '31/03/2017'
      --  and con_pag.cd_previsao is not null
   AND PLANO_CONTAS.CD_CONTABIL between '4.1.01.01.0001' and  '4.3.01.01.9999'
 ORDER BY 5                      ASC,
          3                      ASC,
          2                      ASC,
          9                      ASC,
          ratcon_pag.cd_item_res,
          item_res.ds_item_res,
          con_pag.dt_lancamento,
          setor.nm_setor,
          con_pag.ds_fornecedor
