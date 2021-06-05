---
title: Mesa redonda
author: kov
type: post
date: 2011-05-04T23:40:26+00:00
url: /2011/05/mesa-redonda/
categories:
  - Estupidez humana
  - sysadmin

---
Anos atrás trabalhei num ambiente em que todos os usuários praticamente conheciam o hostname do servidor de arquivos. Usuários saberem hostnames de servidores de arquivos quase nunca é um bom sinal. Todos reclamavam constantemente do tal servidor, falando sempre de como era lento o acesso aos arquivos. Assim que entrei me informaram das medidas que já haviam sido tomadas: sempre que a reclamação ficava muito grande o administrador de rede colocava uma placa de rede de 100Mbps nova no servidor, mas isso parecia pouco resolver.

Alguns anos mais tarde trabalhei em outro lugar em que havia uma aplicação web acessada por um número bastante grande de usuários de todo o Brasil. Havia um grande porém. Apesar de eu fazer parte da equipe de infraestrutura, todo o ambiente de produção dessa instituição ficava sob a responsabilidade de uma outra empresa. A tal aplicação começou a exibir graves problemas de desempenho e como havia prazos relacionados ao preenchimento de dados na aplicação, cada dia que passava sem que muitos usuários conseguissem fazer qualquer coisa no sistema gerava um aumento considerável na preocupação dos gestores.

Ao contrário do administrador do primeiro lugar a solução da equipe responsável pela TI dessa segunda instituição era menos mão-na-massa: fazíamos reuniões em torno de uma mesa em que eram debatidas as diversas possibilidades e sugeridas possíveis soluções. Alguns achavam que o problema era que a aplicação era lenta, outros que o servidor web estava mal configurado, outros que era falta de banda. Houve quem sugerisse que se substituísse a aplicação web por uma aplicação que rodasse na máquina dos usuários, com acesso direto ao banco de dados, o que supostamente reduziria o uso de banda.

O que há de errado em ambas as histórias? Ninguém nunca havia pensado em coletar dados, medir, **saber** o que havia de errado para depois atacar o problema. Eles preferiam fazer o pior tipo de tratamento de problemas de desempenho: imaginar qual seria o problema e chutar soluções que pudessem saná-los.

No primeiro caso, logo depois que entramos eu e um colega, a primeira coisa que esse colega fez foi instalar um monitoramento razoavelmente completo de indicadores de desempenho de todo o ambiente de rede com o [cacti][1]. Não demorou muito para que nós descobríssemos que o problema do nosso querido servidor de arquivos estava longe de ser banda. A carga de transferência de dados nunca passava dos 30Mbps, o que é muito menos que os 100Mbps que uma única placa de rede comum à época conseguiria de desempenho nominal. Adicionar placas de rede não adiantava absolutamente nada. Nem me lembro exatamente qual era o problema, provavelmente uma soma de fatores que incluíam uma configuração pra lá de absurda do Samba e uma configuração de raid que não fazia sentido, mas isso não vem ao caso. O importante dessa história é que o remédio que estava sendo dado para nosso paciente não ajudava em nada a melhorar a doença que ele tinha ;).

O segundo caso é um pouco mais triste: depois de algumas semanas tentando brigar com a estrutura burocrática para nos permitir acessar os servidores e saber o que estava acontecendo chegamos a um indicador bastante interessante: a [carga média][2] de 1 minuto do servidor de banco de dados estava em torno de 70. A máquina se não me engano tinha somente um processador. A carga média no Linux indica o número de processos que estão na fila para usar o processador ou bloqueados por espera de I/O, em médias de 1, 5 e 15 minutos. Como nós já havíamos investigado (e até tomado o controle) dos servidores de aplicação, nossa investigação sobre qual era o gargalo naquele momento parecia ter chegado a uma conclusão: o banco de dados e/ou o que a aplicação pedia dele precisava de amor e carinho. Acontece que não adiantou nada nós termos descoberto isso: continuamos por mais algumas semanas com todo mundo correndo em círculos como galinhas sem cabeça e sem ninguém com conhecimentos adequados chegar perto de analizar o banco de dados e as consultas feitas, até que finalmente alguém mais de cima decidiu mandar todos os nossos chefes embora e fazer uma intervenção.

Depois de algumas semanas batendo cabeça do mesmo jeito, com mais reuniões inúteis em torno de mesas (mas dessa vez com outros personagens) finalmente conseguimos que alguém com poder de agir nos escutasse e consertasse o banco de dados, embora nossa demanda por melhorias na aplicação (que nossas pesquisas paralelas e sem nenhuma ajuda do DBA indicaram colocar mais peso no banco do que o necessário) tenham caído em ouvidos surdos. As mudanças feitas no banco (otimização de queries, criação de índices e views materializadas, essencialmente, pelo que minha leiguice captou) acabaram dando gás para que o sistema sobrevivesse mais algum tempo sem uma intervenção mais estrutural, que acabamos fazendo quando finalmente assumimos o controle do servidor de banco de dados também, mas essa já é uma história pra outro post de blog.

Problemas de desempenho e otimizações são aquele tipo de coisa em que a pior coisa que você pode fazer é sentar em volta de uma mesa e tentar tirar qual o problema do ar. Agir com soluções de senso comum também só dá certo se você for sortudo. Esse assunto me interessa e por isso estou pensando em fazer alguns posts falando de problemas de desempenho e otimizações prematuras que demonstram que nem sempre o que é intuitivo é a realidade e que mesmo que você tenha uma hipótese bastante plausível é melhor investigar antes de torná-la uma crença. Alguém tem histórias interessantes pra compartilhar? =)

**Nota:** essa é uma obra de ficção baseada em fatos reais; algumas simplificações foram feitas sem prejuízo do caso em geral

 [1]: http://www.cacti.net/
 [2]: http://en.wikipedia.org/wiki/Load_(computing)