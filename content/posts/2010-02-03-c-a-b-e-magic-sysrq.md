---
title: C-A-B e Magic SysRq
author: kov
type: post
date: 2010-02-03T12:04:33+00:00
url: /2010/02/c-a-b-e-magic-sysrq/
categories:
  - Nada

---
Isso era pra ser comentário de um post no [blog do Eriberto][1], mas como ele exige login para fazer comentários e reiniciar a senha que eu obviamente esqueci demorou muito, vai um post mesmo.

O post dele trata da ida embora da combinação Control-Alt-Backspace, que matava o X. Essa combinação ir embora é uma coisa boa, na minha opinião por vários motivos, o principal deles sendo que eu já derrubei meu X várias vezes sem querer fazendo um comando no Emacs ou no Bash ;D. Se uma combinação desse tipo era tão importante e precisava ser intuitiva é melhor a gente parar de zoar a Microsoft fazendo camisas com C-A-del e começar a zoar a nós mesmos =P.

Depois ele diz que &#8220;Novidade: agora é AltGR PrintScreen K.&#8221;. Não é bem assim. Essa combinação existe desde sempre e é um dos comandos do chamado Magic SysRq do Linux. Essa combinação específica serve para matar todos os processos do virtual terminal atual, que acaba por ser o suficiente para conseguir algo semelhante ao C-A-B. Outras combinações são AltGr+SysRq+e, que manda um SIGTERM pra todo mundo, AltGr+SysRq+i, que manda um SIGKILL pra moçada, AltGr+SysRq+s que faz um sync de emergência (manda pro disco tudo que tá em memória esperando pra ir pro disco), AltGr+SysRq+u, que remonta os sistemas de arquivo em modo leitura, e AltGr+SysRq+b que dá reboot.

As combinações mágicas precisam estar habilitadas no Linux (estão na maioria das distribuições, por padrão) e podem te ajudar a sair de um &#8220;travamento&#8221;, mesmo que seja reiniciando o sistema de forma limpa, sem risco de perder dados.

 [1]: http://www.eriberto.pro.br/blog/?p=208