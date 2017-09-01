SELECT -- P_Unidade
 --, P_Linha
 '<span style="font-size: 14">'||Leito||'</span>' Leito
 , '<span style="font-size: 14"><strong>'||Paciente||'</strong></span>' Paciente 
 , '<span style="font-size: 14">'||DT_NASCIMENTO||'</span>' DT_NASCIMENTO
 , '<span style="font-size: 14">'||IDADE||'</span>' IDADE
 , '<span style="font-size: 14">'||Atendimento||'</span>' Atendimento
 , '<span style="font-size: 14">'||Convenio||'</span>' Convenio
 , '<span style="font-size: 14">'||Medico||'</span>' Medico 
 , '<span style="font-size: 14">'||Dt_Internacao||'</span>' Dt_Internacao
 , '<span style="font-size: 14">'||Dias||'</span>' Dias

 , BLC BLC
 , EVM EVM
 , EVE EVE
 , PRM PRM
 , NUT NUT
 , FIS FIS 
-- , PRA
-- , CKG
 , '<span style="font-size: 14">'||PXH||'</span>' PXH 
-- , APZ 
-- , TEV
-- , AR
-- , CTO
-- , GOT
 , ALE ALE
-- , BLH
-- , MON
 , EXA EXA
 , IMG IMG
-- , PED PED
-- , DEV
 , ALT ALT
-- , Qtde
 FROM (
 select P_Unidade
 , ROWNUM P_Linha
 , Leito
 , Paciente
 , Atendimento
 , DT_NASCIMENTO
 , IDADE
 , Medico
 , Convenio
 , Dt_Internacao
 , Dt_Prev_Alta
 , Dias
 , Decode(Paciente,null,null,Decode(AgendaBlocoCirurgico,1,'<img src="imagens/setaDireita.png" >',null)) BLC
 , Decode(Paciente,null,null,Decode(AgendaHemodinamica,1,'<img src="imagens/setaDireita.png" >',null)) HMD
 , Decode(Paciente,null,null,Decode(AgendaEndoscopia,1,'<img src="imagens/setaDireita.png" >',null)) END
 , Decode(Paciente,null,null,Decode(EvolucaoMedica,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/evomedica.png" >')) EVM
 , Decode(Paciente,null,null,Decode(EvolucaoEnfermagem,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/evoenfermagem2.png" >')) EVE
 , Decode(Paciente,null,null,Decode(PrescricaoMedica,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/premed.png" >')) PRM
 , Decode(Paciente,null,null,Decode(PrescricaoNut,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/evonutricao.png">')) NUT
 , Decode(Paciente,null,null,Decode(PrescricaoFisio,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/evofisio.png">')) FIS
 , Decode(Paciente,null,null,Decode(PrescricaoAberta,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) PRA
 , Decode(Paciente,null,null,Decode(ChecagemMedicacao,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) CKG
 , Decode(Paciente,null,null,Proximohorario) PXH
 , Decode(Paciente,null,null,Decode(Aprazamento,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) APZ
 , Decode(Paciente,null,null,Decode(ProtocoloTev,1,'<img src="imagens/Documentopreenchido.gif" >',null)) TEV
 , Decode(Paciente,null,null,Decode(PrecaocaoAr,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) AR
 , Decode(Paciente,null,null,Decode(PrecaucaoContato,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) CTO
 , Decode(Paciente,null,null,Decode(PrecaucaoGoticula,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) GOT
 , Decode(Paciente,null,null,Decode(AvisoAlergia,1,'<img src="imagens/vermelho2.gif" >')) ALE
 , Decode(Paciente,null,null,Decode(Monitoramento,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) MON
 , Decode(Paciente,null,null,Decode(BalancoHidrico,1,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) BLH
 , Decode(Paciente,null,null,Decode(ResultadoExames,1,'<img src="imagens/resultadoExame.png" >',null)) EXA
 , Decode(Paciente,null,null,Decode(ResultadoImagens,1,'<img src="imagens/resultadoImagem.png" >',null)) IMG
 , Decode(Paciente,null,null,Decode(FarmaciaPedidos,1,'<img src="imagens/estoque.png" >',2,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) PED
 , Decode(Paciente,null,null,Decode(FarmaciaDevolucao,1,'<img src="imagens/estoque.png" >',2,'<img src="imagens/vermelho2.gif" >','<img src="imagens/verde.png" >')) DEV
 , Decode(AltaMedica,1,'<img src="imagens/ico_altamedica.gif >',Decode(TP_OCUPACAO,'O','<img src="imagens/ico_leitoocupado.png" >','V','<img src="imagens/ico_leitovago.png" >','L','<img src="imagens/ico_limpeza.png" >','R','<img src="imagens/ico_reserva.png">','M','<img src="imagens/ico_manutencao.png" >')) ALT
 , Decode(Paciente,null,0,1) Qtde 
 From (
 select Leitos.Cd_Unid_Int P_Unidade
 , Leitos.Ds_resumo Leito
 , Movimento.Atendimento
 , Movimento.Paciente
 , Movimento.DT_NASCIMENTO
 , Movimento.IDADE
 , Movimento.Medico
 , Movimento.Convenio
 , Movimento.Dt_Internacao
 , Movimento.Dt_Prev_Alta
 , Movimento.Dias
 , Movimento.AgendaBlocoCirurgico
 , Movimento.AgendaHemodinamica
 , Movimento.AgendaEndoscopia
 , Movimento.ChecagemMedicacao
 , Movimento.FarmaciaPedidos
 , Movimento.PrescricaoMedica
 , Movimento.PrescricaoAberta
 , Movimento.ProximoHorario
 , Movimento.Aprazamento
 , Movimento.EvolucaoMedica
 , Movimento.EvolucaoEnfermagem
 , Movimento.ProtocoloTev
 , Movimento.PrecaucaoGoticula
 , Movimento.PrecaucaoContato
 , Movimento.PrecaocaoAr
 , Movimento.AvisoAlergia
 , Movimento.ResultadoExames
 , Movimento.ResultadoImagens
 , Movimento.FarmaciaDevolucao
 , Movimento.BalancoHidrico
 , Movimento.Monitoramento
 , Movimento.AltaMedica
 , Movimento.cd_atendimento
 , Leitos.Tp_Ocupacao
 , Leitos.Cd_Leito_Aih
 , Movimento.PrescricaoNut
 , Movimento.Nutricionista 
 , Movimento.PrescricaoFisio 
 from (
 SELECT Decode(Substr(Paciente.nm_paciente,40,41),null,Paciente.nm_paciente,Substr(Paciente.nm_paciente,1,20)||'...') Paciente 
 , LPad(atendime.cd_atendimento,8,0) Atendimento
 , Decode(Substr(Prestador.nm_prestador,40,41),null,prestador.nm_prestador,Substr(prestador.nm_prestador,1,20)||'...') Medico 
 , Decode(Substr(Convenio.Nm_Convenio,9,10),NULL,Convenio.Nm_Convenio,Substr(Convenio.Nm_Convenio,1,20)||'...') Convenio
 , To_Char(paciente.DT_NASCIMENTO, 'dd/mm/yyyy') DT_NASCIMENTO
 , ROUND((sysdate - PACIENTE.Dt_NASCIMENTO )/365) IDADE 
 , To_Char(Atendime.Dt_Atendimento,'dd/mm/yy')||' '||To_Char(Atendime.Hr_Atendimento,'hh24:mi') Dt_Internacao
 , To_Char(Atendime.Dt_Prevista_Alta,'dd/mm/yy') Dt_Prev_Alta
 , trunc(sysdate-Atendime.dt_atendimento) Dias
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'CHECAGEMMEDICACAO') ChecagemMedicacao
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PROXIMOHORARIO') ProximoHorario
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'APRAZAMENTO') Aprazamento
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'FARMACIAPEDIDOS') FarmaciaPedidos
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PRESCRICAOMEDICA') PrescricaoMedica
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PRESCRICAOABERTA') PrescricaoAberta
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'EVOLUCAOMEDICA') EvolucaoMedica
 , dbamv.hur3_fnc_painel_postos(cd_atendimento,'PRESCRICAONUT') PrescricaoNut 
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'EVOLUCAOENFERMAGEM') EvolucaoEnfermagem
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PROTOCOLOTEV') ProtocoloTev
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'AGENDABLOCOCIRURGICO') AgendaBlocoCirurgico
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'AGENDAHEMODINAMICA') AgendaHemodinamica
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'AGENDAENDOSCOPIA') AgendaEndoscopia
 , dbamv.hur3_fnc_painel_postos(cd_atendimento,'NUTRICIONISTA') Nutricionista 
 , dbamv.hur3_fnc_painel_postos(cd_atendimento,'PRESCRICAOFISIO') PrescricaoFisio 
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PRECAUCAOGOTICULA') PrecaucaoGoticula
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PRECAUCAOCONTATO') PrecaucaoContato
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'PRECAUCAOAR') PrecaocaoAr
 , Nvl(dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'AVISOALERGIADOC'),dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'AVISOALERGIATELA')) AvisoAlergia
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'RESULTADOEXAMES') ResultadoExames
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'FARMACIADEVOLUCAO') FarmaciaDevolucao
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'BALANCOHIDRICO') BalancoHidrico
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'ALTAMEDICA') AltaMedica
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'RESULTADOIMAGENS') ResultadoImagens
 , dbamv.HUR3_Fnc_Painel_Postos(cd_atendimento,'MONITORAMENTO') Monitoramento
 , Atendime.cd_atendimento Cd_atendimento
 , Leito.Cd_Leito_Aih Cd_Laito_Aih
 , Leito.Cd_leito Cd_leito
 FROM Dbamv.Atendime
 , Dbamv.Leito
 , Dbamv.Paciente
 , Dbamv.Prestador
 , Dbamv.Convenio
 where Atendime.Cd_Leito = Leito.Cd_Leito
 and Atendime.Cd_Paciente = paciente.cd_paciente
 and atendime.cd_prestador = prestador.cd_prestador
 and Atendime.Cd_convenio = Convenio.Cd_Convenio
 and Atendime.Tp_Atendimento = 'U'
 and Atendime.Dt_Alta is null
 and Leito.Cd_Unid_Int in ('7','8')
 Order by Leito.Cd_Leito_Aih
 ) Movimento
 , Dbamv.Leito Leitos
 where Leitos.Cd_leito = Movimento.Cd_leito(+)
 and Leitos.Dt_Desativacao is null
 )
 WHERE P_Unidade IN ('1') 
 )

order by leito



