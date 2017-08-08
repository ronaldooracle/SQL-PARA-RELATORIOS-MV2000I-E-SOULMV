SELECT b.cd_atendimento,
       b.nr_chamada_painel,
       to_char(a.dh_processo, 'hh24:mi:ss') as hr_Da_senha,
       to_char(b.hr_atendimento, 'hh24:mi:ss') as Hr_do_atend,
	--- 60 segundos * 60 minutos = 3600 * 24 um dia = 86400 / 3600 = horas
	--- 60 segundos * 60 minutos = 3600 * 24 um dia = 86400 / 3600 / 60 Minutos = Minutos
	--- 60 segundos * 60 minutos = 3600 * 24 um dia = 86400 / 3600 / 60 Segundos = Segundos
       trunc(((b.hr_atendimento - a.dh_processo) * 86400 / 3600)) || ':' ||
       trunc(mod((b.hr_atendimento - a.dh_processo) * 86400, 3600) / 60) || ':' ||
       trunc(mod(mod((b.hr_atendimento - a.dh_processo) * 86400, 3600), 60)) Tempo
  from dbamv.sacr_tempo_processo a, atendime b
 where a.nm_MAQUINA = 'SENHA'
   and b.cd_atendimento = a.cd_atendimento
