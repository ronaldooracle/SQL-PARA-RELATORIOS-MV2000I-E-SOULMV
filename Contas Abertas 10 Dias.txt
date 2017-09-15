select decode(atendime.tp_atendimento,
              'I',
              'Internacao',
              'U',
              'Urgencia',
              'A',
              'Ambulatorio',
              'E',
              'Externo') as Tipo_Atend,
       DBAMV.REG_FAT.CD_ATENDIMENTO,
       DBAMV.REG_FAT.CD_REG_FAT,
       paciente.nm_paciente,
       atendime.dt_alta,
       (round((sysdate - atendime.dt_alta) * 24 / 24, 0)) Dias,
       
       case DBAMV.REG_FAT.SN_FECHADA
         when 'N' then
          'Aberto'
         else
          'Fechado'
       end Status_Conta
  from DBAMV.REG_FAT, atendime, paciente
 where dbamv.Reg_Fat.sn_fechada = 'N'
   and atendime.cd_atendimento = DBAMV.REG_FAT.CD_ATENDIMENTO
   and paciente.cd_paciente = atendime.cd_paciente
   and atendime.dt_alta is not null
   and (round((sysdate - atendime.dt_alta) * 24 / 24, 0)) <= 10 --##### Informar Dias #####

UNION ALL

select decode(atendime.tp_atendimento,
              'I',
              'Internacao',
              'U',
              'Urgencia',
              'A',
              'Ambulatorio',
              'E',
              'Externo') as Tipo_Atend,
       DBAMV.Itreg_Amb.CD_ATENDIMENTO,
       DBAMV.Itreg_Amb.CD_REG_amb,
       paciente.nm_paciente,
       atendime.dt_alta,
       (round((sysdate - atendime.dt_alta) * 24 / 24, 0)) Dias,
       
       case DBAMV.Itreg_Amb.SN_FECHADA
         when 'N' then
          'Aberto'
         else
          'Fechado'
       end Status_Conta
  from DBAMV.Itreg_Amb, atendime, paciente
 where dbamv.Itreg_Amb.sn_fechada = 'N'
   and atendime.cd_atendimento = DBAMV.Itreg_Amb.CD_ATENDIMENTO
   and paciente.cd_paciente = atendime.cd_paciente
   and atendime.dt_alta is not null
   and (round((sysdate - atendime.dt_alta) * 24 / 24, 0)) <= 10 --##### Informar Dias #####
