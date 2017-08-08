SELECT
      prestador_aviso.sn_principal ,
      PRESTADOR.NM_PRESTADOR,
      A.CD_ATENDIMENTO AS ATENDIMENTO,
      B.CD_CONVENIO,E.NM_CONVENIO,
       A.CD_AVISO_CIRURGIA AS AVISO,
       A.NM_PACIENTE AS PACIENTE,
       C.DS_CIRURGIA AS CIRURGIA,
       D.DS_SAL_CIR AS SALA,
       A.DT_REALIZACAO AS INICIO,
       A.DT_SAIDA_SAL_CIR AS FIM,
       To_Char(A.VL_TEMPO_DURACAO,'HH24:MI') AS TEMPO,
       SUM(round(((SUBSTR(To_char(A.VL_TEMPO_DURACAO,'HH24:MI'),1,2)*60)+SUBSTR(To_char(A.VL_TEMPO_DURACAO,'HH24:MI'),4))/60,2)) over() total,
       f.vl_servico_profissional,
       c.cd_procedimento_sih,
       f.dt_vigencia
    FROM DBAMV.AVISO_CIRURGIA A,
        DBAMV.CONVENIO E ,
       DBAMV.CIRURGIA_AVISO B,
       DBAMV.CIRURGIA C,
       DBAMV.SAL_CIR D ,DBAMV.PROCEDIMENTO_SUS_VALOR f,
        prestador_aviso,
        PRESTADOR
 WHERE A.CD_AVISO_CIRURGIA = B.CD_AVISO_CIRURGIA
   and B.CD_CIRURGIA = C.CD_CIRURGIA
   and f.cd_procedimento = c.cd_procedimento_sih
   and A.CD_SAL_CIR = D.CD_SAL_CIR
   AND PRESTADOR.CD_PRESTADOR = PRESTADOR_AVISO.CD_PRESTADOR
AND PRESTADOR_AVISO.CD_CIRURGIA_AVISO = B.CD_CIRURGIA_AVISO
   and prestador_aviso.Cd_Aviso_Cirurgia = B.CD_AVISO_CIRURGIA
   AND prestador_aviso.Cd_Prestador = PRESTADOR.CD_PRESTADOR
   and A.TP_SITUACAO = 'R'
   AND E.CD_CONVENIO = B.CD_CONVENIO
 AND f.dt_vigencia IN (SELECT MAX(DISTINCT(N.DT_VIGENCIA)) FROM DBAMV.PROCEDIMENTO_SUS_VALOR N)
AND TRUNC(dt_inicio_cirurgia) BETWEEN  '01/09/2016' AND '28/02/17'
order by cd_atendimento			
