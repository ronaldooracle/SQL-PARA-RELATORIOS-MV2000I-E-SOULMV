select A.CD_PED_LAB,
       C.CD_ATENDIMENTO,
       D.NM_PACIENTE,
       A.HR_PED_LAB,
       E.NM_PRESTADOR,
       G.DS_UNID_INT,
       F.DS_LEITO,
   case when  round(sysdate - a.hr_ped_lab) > 50 then 'Maior 50 Dias<img src="imagens/vermelho3.gif" >'
     when  round(sysdate - a.hr_ped_lab) < 51 then 'Menor 49 Dias<img src="imagens/situacaoAmarela.gif" >'
     else ''
       end Status

  from DBAMV.PED_LAB A,
       ATENDIME      C,
       PACIENTE      D,
       PRESTADOR     E,
       LEITO         F,
       UNID_INT      G
 where A.CD_PED_LAB in (select B.CD_PED_LAB
                          from DBAMV.ITPED_LAB B
                         where B.CD_LABORATORIO = '1')
   AND C.CD_ATENDIMENTO = A.CD_ATENDIMENTO
   and A.CD_PED_LAB IN (SELECT COLETA_MATERIAL.CD_PED_LAB
                          FROM COLETA_MATERIAL, AMOSTRA
                         WHERE COLETA_MATERIAL.CD_COLETA_MATERIAL =
                               AMOSTRA.CD_COLETA_MATERIAL
                           AND AMOSTRA.SN_COLETA = 'N')
   AND D.CD_PACIENTE = C.CD_PACIENTE
   AND E.CD_PRESTADOR = A.CD_PRESTADOR
   AND F.CD_LEITO = C.CD_LEITO
   AND TRUNC(A.DT_PEDIDO) = sysdate
   AND G.CD_UNID_INT = F.CD_UNID_INT
 ORDER BY 6 DESC

