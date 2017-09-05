SELECT 
       CD_ESTOQUE,
       DT_REALIZACAO,
       CD_ATENDIMENTO,
       DT_CONSUMO_PACIENTE,
       DSP_NM_PACIENTE,
       HR_CONSUMO_PACIENTE,
       CD_USUARIO,
       DT_MVTO,
       SN_CONFIRMA_CONSIGNADO,
       CD_CONSUMO_PACIENTE,
       CD_CONVENIO
  FROM (select distinct nvl(mvto.cd_aviso_cirurgia, 0) cd_aviso_cirurgia,
                        trunc(avi_cir.dt_realizacao) dt_realizacao,
                        mvto.cd_atendimento cd_atendimento,
                        atendime.cd_paciente cd_paciente,
                        paciente.nm_paciente dsp_nm_paciente,
                        null dt_consumo_paciente,
                        null hr_consumo_paciente,
                        null cd_usuario,
                        'N' sn_confirma_consignado,
                        null cd_consumo_paciente,
                        atendime.cd_convenio,
                        convenio.nm_convenio nm_convenio,
                        mvto.cd_estoque,
                        mvto.dt_mvto_estoque DT_MVTO
          from dbamv.itmvto_estoque itmvto,
               dbamv.mvto_estoque mvto,
               (select cd_aviso_cirurgia, dt_realizacao, cd_atendimento
                  from dbamv.aviso_cirurgia
                 where tp_situacao in ('R', 'G', 'T', 'A')) avi_cir,
               dbamv.convenio convenio,
               dbamv.Empresa_Convenio,
               dbamv.atendime,
               dbamv.paciente,
               dbamv.produto prd
         where mvto.cd_mvto_estoque = itmvto.cd_mvto_estoque
           and mvto.cd_aviso_cirurgia = avi_cir.cd_aviso_cirurgia(+)
           and mvto.cd_atendimento = atendime.cd_atendimento
           and atendime.cd_paciente = paciente.cd_paciente(+)
           and atendime.cd_convenio = convenio.cd_convenio
           and Empresa_Convenio.Cd_Convenio = Convenio.Cd_Convenio
           and itmvto.cd_produto = prd.cd_produto
           and prd.sn_consignado = 'S'
           And atendime.cd_multi_empresa = 1
           And Empresa_Convenio.Cd_Multi_Empresa = 1
           and not exists
         (select 1
                  from dbamv.consumo_paciente cons1
                 where cons1.cd_atendimento = mvto.cd_atendimento
                   and cons1.cd_aviso_cirurgia = mvto.cd_aviso_cirurgia)
           and Exists
         (Select mvto_estoque.cd_atendimento,
                       itmvto_estoque.cd_produto,
                       sum(decode(tp_mvto_estoque, 'P', 1, -1) *
                           itmvto_estoque.qt_movimentacao *
                           verif_vl_fator_prod(itmvto_estoque.cd_produto)) /
                       verif_vl_fator_prod(itmvto_estoque.cd_produto) qt_consumo
                  from dbamv.mvto_estoque,
                       dbamv.itmvto_Estoque,
                       dbamv.produto
                 where mvto_estoque.cd_mvto_estoque =
                       itmvto_estoque.cd_mvto_estoque
                   and mvto_estoque.cd_atendimento = mvto.cd_atendimento
                   and mvto_estoque.cd_estoque = mvto.cd_estoque
                   and itmvto_estoque.cd_produto = produto.cd_produto
                   and produto.sn_consignado = 'S'
                   and mvto_estoque.cd_estoque in
                       (select usu_estoque.cd_estoque
                          from dbamv.usu_estoque usu_estoque
                         where usu_estoque.tp_usuario <> 'S'
                           and usu_estoque.cd_estoque IN
                               (select cd_estoque
                                  from dbamv.estoque
                                 where cd_multi_empresa =1 )
                           AND usu_estoque.cd_id_do_usuario =
                               (select username from user_users))
                 Group by mvto_estoque.cd_atendimento,
                          itmvto_estoque.cd_produto
                Having sum(decode(tp_mvto_estoque, 'P', 1, -1) * itmvto_estoque.qt_movimentacao * verif_vl_fator_prod(itmvto_estoque.cd_produto)) / verif_vl_fator_prod(itmvto_estoque.cd_produto) > 0))
                where trunc(DT_MVTO) between '01/04/2017' and '30/04/2017'
