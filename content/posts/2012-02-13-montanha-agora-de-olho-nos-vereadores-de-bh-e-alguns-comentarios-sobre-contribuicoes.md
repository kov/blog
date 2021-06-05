---
title: 'Montanha: agora de olho nos vereadores de BH e alguns comentários sobre contribuições'
author: kov
type: post
date: 2012-02-13T19:36:18+00:00
url: /2012/02/montanha-agora-de-olho-nos-vereadores-de-bh-e-alguns-comentarios-sobre-contribuicoes/
categories:
  - development
  - governo

---
Alguns dos leitores talvez saibam que eu escrevi no meio de 2010 um programa chamado &#8216;Montanha&#8217;. A ideia original do programa era me ajudar a escolher um candidato a deputado estadual me dando uma ideia geral de como os deputados da época gastavam os recursos da verba indenizatória. O site da Assembléia Legislativa de Minas Gerais publica essas informações, mas de uma forma muito inconveniente, tornando absolutamente impossível ter uma idea geral de como os deputados gastam a bufunfa. Obviamente botei o [código online][1] e subi uma [instância pública][2] para que outras pessoas pudessem fazer o mesmo. Depois disso o grande tevaum se juntou ao time e já adicionamos uma instância para a [nova legislatura][3], que tomou posse em 2011.

Nos últimos dias decidi que com os belorizontinos prestando atenção nos vereadores, dada a polêmica sobre o aumento de salários e o veto pelo prefeito, seria um bom momento para [criar o coletor][4] e subir uma instância nova do Montanha, pra [observar os gastos dos vereadores][5]. Quem olhar vai notar rapidamente que o projeto ainda está pela metade: ainda faltam informações de partido dos vereadores, os links falam em &#8216;deputados&#8217; e por aí vai, mas sou fiel ao princípio de _release early, release often_, então não quis esperar &#8211; quando os dados começaram a encher o banco botei o projeto pra fora.

Agora alguns comentários sobre questões que as pessoas me colocam:

**Bacana! Se precisar de ajuda tamos aí!**

Obrigado! Esse é um projeto de software livre &#8211; o código está sob a Affero GPL3 e sua contribuição é bem-vinda. Eu acredito firmemente em outro princípio: _talk is cheap; show me the code_. Eu não pretendo organizar/coordenar os esforços de outras pessoas, então não espere que eu peça ajuda para algo específico ou pegue na mão, sinta-se à vontade para clonar o projeto, fazer as modificações que achar que devem ser feitas e propô-las, não posso garantir que alguma coisa será incorporada ao meu branch, mas estou disposto a discutir questões de design/planos e responder dúvidas sobre o código &#8211; no canal #linux-bh da freenode, principalmente =).

**Quais os planos pro futuro?**

O meu TODO imediato é (e sinta-se à vontade pra roubar qualquer um e fazer):

  * colocar os dados de partido nos dados da Câmara Municipal de BH
  * mudar a interface do montanha para não falar em &#8216;deputados&#8217;, mas em &#8216;parlamentares&#8217;
  * escrever um coletor para os dados anteriores a março de 2010 da CMBH
  * melhorar a linkabilidade das pesquisas &#8211; deixar que você envie um link da visão de todos os gastos, por exemplo, com uma busca já feita
  * escrever alguns posts no Observador Político e no Trezentos chamando a atenção para algumas informações expostas pelo montanha
  * adicionar mais gráficos &#8211; gasto sobre tempo, por exemplo
  * melhorar a informação que o sistema dá a respeito do período coberto pelos dados
  * aumentar a quantidade de trivia exibida na página de detalhes de parlamentar
  * criar uma página com detalhes e trivia para fornecedores

**Por que você não coloca esse projeto no Transparência Hacker (ou outro grupo)?**

A minha resposta para esse tipo de pergunta tem sido &#8216;por quê eu deveria&#8217;? Não é que eu seja um lobo solitário, mas eu acho que só faz sentido participar de um projeto específico se houver alguma razão para tal. Visibilidade não me preocupa muito &#8211; a mensagem sempre acaba chegando em quem se interessa e em quem me interessa que ela chegue.

Eu não acredito que participar de um grupo &#8211; qualquer grupo &#8211; seja garantia de contribuidores, também; como eu disse, _talk is cheap_ e disso eu tenho certeza de que acharia muito num grupo, mas acredito que as pessoas que quiserem contribuir vão contribuir independente de estar dentro de um grupo (como o Estêvão faz). Se um pedaço grande da contribuição vier de pessoas que fazem parte de um grupo e fizer sentido discutir o projeto dentro dele, aí sim eu veria sentido, por exemplo.

Uma última preocupação, essa específica com o thack, é que o foco do grupo me parece muito diferente do meu. Meu objetivo é que a sociedade tenha uma ferramenta para observar seus parlamentares. Para que isso aconteça é preciso que a ferramenta tenha uma vida mais longa e seja mantida. Os dados sobre a legislatura passada da ALMG, por exemplo, já foram retiradas do site da ALMG, mas o Montanha continua lá, a sociedade continua tendo acesso não só a todos dados, como a uma visualização mais razoável deles. Eu não estou prometendo que vou manter pra sempre, claro, principalmente porque faço isso no meu tempo vago (em que eu também trabalho pro Debian, GNOME, como, durmo e me divirto), mas a minha ideia é focar nesse um problema e ter uma boa solução razoavelmente perene.

A maioria das coisas que eu vi do thack são hacks muito bacanas, mas sua vida parece ser muito curta &#8211; assim que um hack está pronto outra ideia legal aparece e aquela é deixada para trás; essa bola já foi levantada por outras pessoas, inclusive, como exemplo de por quê grupos como o thack não são a solução definitiva para o problema de dados abertos e de por quê concursos de criação de app não substituem um trabalho sério dentro do governo; não é incomum achar coisas com dados de anos atrás ou que sequer continuam funcionando. Note que eu não tenho nada contra o thack, per se, muito menos contra as pessoas que o compõe &#8211; eu os considero colegas e amigos, eu só acho que nós surfamos ondas diferentes e isso me faz achar que eu não agregaria valor ao grupo e vice-versa. Obviamente posso ser convencido do contrário eventualmente =)

 [1]: https://gitorious.org/montanha/montanha
 [2]: http://montanha.kov.eti.br/
 [3]: http://montanha2011.kov.eti.br/
 [4]: https://gitorious.org/montanha/montanha/commits/cmbh
 [5]: http://cmbh.kov.eti.br/