       select leito.ds_resumo AS leito ,
       paciente.nm_paciente AS paciente ,
       paciente.nr_CNS AS Cartao_sus, trunc((months_between(sysdate, dt_nascimento))/12) AS idade,
       atendime.cd_cid ||' '|| cid.ds_cid AS Diagnostico_Inicial,
       to_char(Trunc(atendime.dt_atendimento),'dd/mm/yyyy') AS Data_do_Atendimento,
       to_char((atendime.hr_atendimento),'hh24:mi:ss') AS Hora_do_Atendimento,
       ori_ate.ds_ori_ate AS Origem,
       '' AS Numero_da_Regulacao,
        '' AS Tipo_da_Regulacao,
         '' AS Situacao_da_Regulacao,                                                                           
        atendime.dt_prevista_alta AS Data_Prevista_Alta,
        atendime.dt_alta AS Data_da_Alta,
        Trunc(SYSDATE - atendime.dt_atendimento)*24/24 AS Dias_De_internacao,
        '' AS Pendencia
       FROM atendime  ,unid_int , leito  ,paciente  ,cid ,ori_ate
       WHERE atendime.cd_leito = leito.cd_leito
       AND unid_int.cd_unid_int = leito.cd_unid_int
       AND paciente.cd_paciente = atendime.cd_paciente
       AND atendime.dt_alta IS NULL                                     
       AND atendime.cd_cid (+)= cid.cd_cid
       AND ori_ate.cd_ori_ate = atendime.cd_ori_ate

