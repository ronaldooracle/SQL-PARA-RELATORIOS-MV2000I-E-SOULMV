select doc.cd_protocolo_doc                 NUMERO_PROTOCOLO,
       doc.cd_setor                         SETOR_ORIGEM,
       setor.nm_setor                       NOME_SETOR_ORIGEM,
       doc.cd_setor_destino                 SETOR_DESTINO,
       setor2.nm_setor                      NOME_SETOR_DESTINO,
       To_Char(doc.dt_envio, 'DD/MM/YYYY HH24:MI')      DATA_ENVIO,       
       doc.nm_usuario_envio                 USUARIO_QUE_ENVIOU,
       doc.ds_observacao                    OBSERVACAO_DO_PROTOCOLO,
       itdoc.cd_documento_prot              COD_DOC_PROT,
       documento_prot.ds_documento_prot     DOCUMENTO_PROTOCOLADO,
       decode(atendime.tp_atendimento,
              'I','Hospitalar',
              'H','Hospitalar',
              'Ambulatorial')               TIPO_DO_ATENDIMENTO,
       atendime.cd_atendimento              CODIGO_DO_ATENDIMENTO,
       atendime.cd_paciente                 CODIGO_DO_PACIENTE,
       paciente.nm_paciente                 NOME_DO_PACIENTE,
--       atendime.cd_prestador                PRESTADOR_DO_ATENDIMENTO,
--       prestador.nm_prestador               NOME_DO_PRESTADOR,
       trunc(itdoc.dt_realizacao)           DATA_DE_REALIZACAO,
       to_char(itdoc.hr_realizacao,'hh24:mi')   HORA_DE_REALIZACAO,       
       itdoc.cd_mvto                        CODIGO_DA_MOVIMENTACAO,
       To_Char(itdoc.dt_recebimento, 'DD/MM/YYYY HH24:MI') DATA_RECEBIMENTO,            
       itdoc.nm_usuario_recebimento         USUARIO_QUE_RECEBEU,
       itdoc.dt_devolucao                   DATA_DE_DEVOLUCAO,
       itdoc.nm_usuario_devolucao           USUARIO_QUE_DEVOLVEU
  from dbamv.protocolo_doc doc,
       dbamv.it_protocolo_doc itdoc,
       dbamv.setor,
       dbamv.setor setor2,
       dbamv.documento_prot,
       dbamv.atendime,
       dbamv.paciente
--       dbamv.prestador
 where itdoc.cd_protocolo_doc  = doc.cd_protocolo_doc
   and setor.cd_setor          = doc.cd_setor
   and setor2.cd_setor(+)      = doc.cd_setor_destino
   and itdoc.cd_documento_prot = documento_prot.cd_documento_prot
   and atendime.cd_atendimento = itdoc.cd_atendimento
   and paciente.cd_paciente    = atendime.cd_paciente
--   AND setor2.CD_SETOR IN ('31')
--   and atendime.cd_multi_empresa = 1
   AND DT_ENVIO BETWEEN '01/07/2016' AND '30/09/2016'

   ORDER BY 3 ASC 
