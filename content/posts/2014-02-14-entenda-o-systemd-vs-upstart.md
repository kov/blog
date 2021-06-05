---
title: Entenda o systemd vs upstart
author: kov
type: post
date: 2014-02-15T01:29:21+00:00
url: /2014/02/entenda-o-systemd-vs-upstart/
categories:
  - Debian
  - systemd

---
**_Originalmente publicado [no PoliGNU][1]._**

# Por quê mudar?

Recentemente o projeto Debian passou por um intenso debate para decidir que sistema de inicialização deveria substituir o venerável (porém antiquado) system v como padrão do sistema para a próxima release, codename _jessie_.

A razão para mudar pode não parecer óbvia, especialmente para os muito apegados à ideia de &#8220;UNIX&#8221; ou que simplesmente estão acostumados a como as coisas funcionam hoje, mas fica clara quando se analisa os principais problemas enfrentados por sistemas que ainda usam sistemas de init **system v**. Pra começo de conversa, o init não conhece muitas das novidades tecnológicas que apareceram nas últimas décadas.

Peguemos como exemplo o controle de hardware: um daemon que precise ser iniciado quando um determinado tipo de hardware é plugado não será executado automaticamente pelo init se for plugado, o que exige que além de ter lógica para tratar o caso no boot, também se tenha que ter outros mecanismos para reagir ao _hot-plugging_.

Ser um conjunto de scripts shell também causa uma infinidade de problemas. Como cada script deve reinventar a inicialização do daemon que controla, é bastante comum que eles não sejam 100% resilientes a problemas como deixar processos (principais ou filhos) para trás quando quem administra o servidor pede que o serviço seja parado. Isso acaba exigindo intervenção de quem administra, matando manualmente os processos.

O desempenho também é um problema que passou muito tempo sem solução. Com o aparecimento de computadores com múltiplas CPUs e com espera de rede se tornando um problema foi inventado um sistema de paralelizar a execução dos scripts de inicialização, o que trouxe consigo a enorme complexidade de ter que estabelecer dependências entre os scripts, para garantir que só sejam executados em paralelo scripts que possam sê-lo. Mas isso também é muito mais complexo do que parece &#8211; as dependências em sistemas modernos são muito mais sutis e variáveis do que pode parecer em primeira análise. Além disso, executar cada script desse gera um overhead não desprezível.

Por último, o diagnóstico de falhas de inicialização de serviços é absurdamente complexo; raramente se sabe em que arquivo de log estarão as mensagens relevantes e com alguma frequência sequer haverá logs da falha propriamente dita em algum lugar, restando a quem administra executar o serviço manualmente e ver o que acontece.

Para resolver alguns ou todos esses problemas vieram os novos sistemas de init. Na verdade o Mac OS X saiu na frente com o launchd, mas essa é história pra uma outra oportunidade, comecemos pelo upstart, que nasceu no Ubuntu há alguns anos atrás.

# Upstart

O upstart surgiu quando a principal preocupação das pessoas estava em melhorar o desempenho do boot através de paralelismo. A ideia é substituir a necessidade de estabelecer dependência entre os serviços de forma declarativa e ao invés disso estabelecer condições para que um serviço esteja &#8220;pronto&#8221; para ser executado.

No sistema de dependências inventado para o system v começa pelo objetivo: preciso rodar rodar o procedimento que monta sistemas de arquivo remotos, pra isso preciso rodar as dependências dele que são montar os sistemas de arquivo locais e estabelecer conexão de rede. O upstart inverte essa lógica e começa dos procedimentos que tem nenhuma dependência, o que vai tornando outros procedimentos &#8220;prontos&#8221; e os executa a partir daí.

A ideia é que o sistema de init reage a eventos, que podem ser um serviço estar estabelecido mas também podem ser de outra natureza: um hardware ser plugado, por exemplo. Isso resolve o problema do desempenho, em certa medida e cria um framework que permite ao sistema de init continuar cuidando do sistema enquanto ele estiver ligado, iniciando e parando serviços conforme as condições do sistema mudam.

Há alguns poréns a respeito dessa estratégia, que quem tiver curiosidade achará com facilidade nos frequentes debates travados entre Lennart Poettering e os proponentes do upstart, mas eu queria chamar a atenção para um em particular: usando eventos não existe garantia de que um serviço esteja de fato em execução plena quando o seu procedimento de inicialização conhecido pelo upstart termina &#8211; anote isso mentalmente.

Finalmente, o upstart acaba não resolvendo os problemas de diagnóstico, nem de processos perdidos sendo deixados pelo sistema (embora uma solução que adota a ideia do systemd para isso esteja sendo criada), principalmente por as receitas de init usadas pelo upstart serem ainda scripts shell customizados.

# Systemd

O systemd é criação de Lennart Poettering, que é funcionário da Red Hat e contribuidor do Fedora. Daí muita gente atribuir o systemd à Red Hat, no que se enganam: embora hoje a Red Hat tenha inúmeros projetos com o systemd no centro, a ideia original e o esforço original foram feitos por Lennart em seu próprio tempo e não como um projeto da empresa.

A motivação para sua criação é bem mais ambiciosa que a que originou o upstart: a ideia é que há inúmeras funcionalidades que o kernel Linux que são incríveis e extremamente avançadas e que acabam não sendo usadas a não ser em ambientes muito específicos, porque não há infra-estrutura comum no espaço de usuário para fazer uso da funcionalidade e disponibilizá-la para o resto do sistema.

Um exemplo: cgroups. Os Control Groups são formas de juntar processos em uma embalagem que permite tratar esse grupo de processos como um todo. Esses grupos podem ser usados por exemplo para limitar que partes do sistema os processos que o compõe podem ver, criando uma visão virtual mais limitada do sistema de arquivos, por exemplo, ou limitando as interfaces de rede que os processos podem ver. Também podem ser usados para tornar o agendamento de tempo das tarefas no processador mais adequado ao sistema, ou impor limites de uso de memória, de operações de I/O e assim por diante.

Uma das primeiras funcionalidades trazidas pelo systemd foi exatamente o uso de Control Groups. Cada serviço iniciado pelo systemd o é dentro de um cgroup próprio, o que significa por exemplo que todos os processos criados por aquele serviço podem ser &#8211; com certeza &#8211; terminados quando o serviço é parado. Também significa que apesar de seu apache estar consumindo muito CPU ou memória, seu servidor SSH ainda tem um naco suficiente do tempo de CPU para aceitar sua conexão que será usada para tratar do problema, já que o Linux tenta ser justo com os diferentes cgroups.

Além de tudo isso, o uso de cgroups ainda dá ao systemd a capacidade de garantir que não fiquem processos pra trás quando um serviço é parada: como processos criados por processos que foram colocados num control group também são colocados nesse control group, basta terminar todos os processos do control group para que não sobre nenhum. O pessoal do upstart pensou em adotar essa ideia.

Mas o systemd também atacou o problema do desempenho, de duas formas: a primeira foi a ideia de remover completamente scripts shell da jogada. Cada serviço é especificados num arquivo de configuração chamado &#8220;unit&#8221;, que descreve todas as informações que o init precisa para iniciar e cuidar do processo. Isso retira da equação a necessidade de iniciar um interpretador shell e de executar os complexos scripts usados atualmente com system v.

A segunda foi dando uma solução um pouco mais inusitada à questão das dependências. A ideia é simples: a maioria dos serviços abre sockets de algum tipo para receber pedidos de seus clientes. O X, por exemplo, cria um socket UNIX em forma de arquivo no /tmp. O que o systemd faz é criar todos os sockets dos serviços que controla (eles estão descritos nos arquivos unit) e, quando recebe uma conexão, inicia o serviço e passa pra ele o socket.

O interessante dessa solução é que ela 1) estabelece a relação de dependência na vida real: o serviço é iniciado quando alguém pede e 2) garante que os clientes que forem iniciados vão ter seu primeiro request atendido, não há mais o problema de não saber quando o serviço está plenamente ativo (lembra da nota mental no caso do upstart?).

Para completar, o systemd controla as saídas padrão e de erro dos serviços que inicia e pode facilmente mostrar as últimas linhas de log de um serviço quando sua execução falha, tornando muito simples o diagnóstico:

```
kov@melancia ~&gt; systemctl status mariadb.service
mariadb.service - MariaDB database server
Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled)
Active: active (running) since Seg 2014-02-03 23:00:59 BRST; 1 weeks 3 days ago
Process: 6461 ExecStartPost=/usr/libexec/mariadb-wait-ready $MAINPID (code=exited, status=0/SUCCESS)
Process: 6431 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir %n (code=exited, status=0/SUCCESS)
Main PID: 6460 (mysqld_safe)
CGroup: /system.slice/mariadb.service
        ├─6460 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
        └─6650 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plug...
	Fev 03 23:00:57 melancia mysqld_safe[6460]: 140203 23:00:57 mysqld_safe Logging to /var/log/mariadb/mariadb.log.
	Fev 03 23:00:57 melancia mysqld_safe[6460]: 140203 23:00:57 mysqld_safe Starting mysqld daemon with databas...mysql
	Fev 03 23:00:59 melancia systemd[1]: Started MariaDB database server.
	Hint: Some lines were ellipsized, use -l to show in full.</pre>
```

# A questão de licenciamento

De extrema importância quando falamos de Software Livre são a questão de licença e de copyright do código. O systemd é licenciado sob a LGPL versão 2.1 ou superior, o que permite que programas proprietários sejam linkados com suas bibliotecas. O copyright das contribuições feitas ao código é mantido pelos colaboradores. Um projeto de software livre bastante comum desse ponto de vista.

Já o Upstart, sendo criação da Canonical segue o que tem sido sua política há algum tempo: usa uma licença GPL (versão 2) e exige que contribuidores assinem um Contributor License Agreement compartilhando os direitos de copyright com a Canonical. Com isso, a Canonical fica sendo a única dona do copyright do conjunto e pode fazer por exemplo alterações de licença, ou lançar uma versão proprietária se entender que deve.

Em primeira análise isso não parece ser um problema: de fato, há várias licenças mais permissivas usadas por outros softwares, como as licenças BSD, que permitem às pessoas fecharem o código. A grande questão aqui é que com esse modelo não é qualquer pessoa que pode fazer isso, só a Canonical pode. Essa acaba sendo uma forma de desnivelar as oportunidades artificialmente, criando um monopólio do privilégio de lançar versões proprietárias.

# Futuro

Atualmente dois sistemas importantes usam upstart: Ubuntu e Red Hat Enterprise Linux 6, que é a versão atual suportada da Red Hat. O systemd foi adotado inicialmente pelo Fedora, o que significa que a futura RHEL 7 também estará usando systemd. Algumas outras distribuições passaram a usar systemd ou como padrão ou como opção bem suportada, como o SuSE e o Arch Linux, por exemplo.

Com a adoção dele pelo Debian como padrão, todas as grandes distribuições base passarão a ser baseadas no systemd. Para surpresa de muitos (minha inclusive), Mark Shuttleworth [anunciou](http://www.markshuttleworth.com/archives/1316) que dada a decisão do projeto Debian, o Ubuntu também passaria a adotar systemd por padrão, aceitando graciosamente a derrota, em suas próprias palavras.

Com essa mudança é muito improvável que o upstart tenha um futuro depois de sua aposentadoria nas versões estáveis do RHEL e Ubuntu LTS, a menos que alguma outra distribuição se apresente para assumir sua manutenção.

Vida longa ao systemd!

&nbsp;

_Nota: ao tratar do CLA exigido pela Canonical o texto anteriormente usava a expressão &#8220;transferindo o copyright&#8221; , que dá a entender que o autor original da contribuição perdia os direitos, o que não é verdade e foi corrigido._

 [1]: http://polignu.org/artigo/entenda-o-systemd-vs-upstart