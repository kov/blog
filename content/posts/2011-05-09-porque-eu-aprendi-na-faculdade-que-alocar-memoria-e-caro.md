---
title: “Porque eu aprendi na faculdade que alocar memória é caro”
author: kov
type: post
date: 2011-05-09T23:39:34+00:00
url: /2011/05/porque-eu-aprendi-na-faculdade-que-alocar-memoria-e-caro/
categories:
  - desempenho
  - development

---
Mais um exemplo interessante a respeito daquilo que falei num [post anterior][1]. Recentemente uma pessoa que começou a trabalhar em um projeto em que eu trabalho há algum tempo me pediu que opinasse a respeito de um patch dele. O patch original dele havia sido revisado por um colega de projeto e além de umas bobagenzinhas de estilo havia uma única preocupação: &#8220;por quê você quer fazer essa mudança?&#8221; A mudança era bastante simples: evitava que uma variável fosse liberada e realocada em alguns casos específicos.

Nesse projeto uma das coisas que as pessoas menos gostam é de otimizações cegas. Se você faz uma mudança, essa mudança tem chances de introduzir bugs e de aumentar a complexidade do código. Se você vai incorrer nesse risco, é melhor saber que está de fato fazendo alguma diferença. Por isso, otimizações em geral só são bem-vindas se forem acompanhadas de um teste de desempenho que mostra melhoria. Se não melhora nada, pra que mudar? É claro que há excessões à regra e algumas mudanças acabam simplificando o código e são bem-vindas mesmo se o único resultado for não piorar o desempenho.

Quando eu falei que não é legal fazer otimizações cegas ele me disse: &#8220;Cega ou não, estou fazendo a mudança porque aprendi na faculdade que alocações são caras, principalmente porque causam chamadas de sistema e por isso devem ser evitadas&#8221;. Opa. Alocações de memória de fato envolvem o kernel, mas será que _todas_ as alocações de memória causam chamadas de sistema e a consequente (e cara) troca de contexto para modo kernel?

Fácil testar isso. Se for verdade que cada alocação gera uma chamada de sistema o seguinte programa vai exibir 100 milhões de chamadas de sistema quando o executarmos através do strace:

```C
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv)
{
    int i;
    char* data;

    printf("START\n");
    for (i = 0; i < 100000000; i++)
        data = malloc(10);
    printf("END\n");

    return 0;
}
```

Vejamos:

```
[...]
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 2), ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbe62175000
write(1, "START\n", 6START
)                  = 6
brk(0)                                  = 0x1b83000
brk(0x1ba4000)                          = 0x1ba4000
write(1, "END\n", 4END
)                    = 4
exit_group(0)                           = ?
```

Mas hein? Duas chamadas de sistema entre os dois writes? Pois é. Acontece que o pessoal que escreveu a malloc() já sabe que pedir memória pro kernel é caro e fizeram a libc pré-alocar uma quantidade maior de memória de uma vez e ir entregando pedaços dessa memória conforme a aplicação pede. Isso significa que alocação de memória não é cara? Não. É razoavelmente cara mesmo que não haja troca de contexto, afinal de contas a libc precisa fazer um tanto de trabalho pra saber quanto tem alocado e saber qual o tamanho de cada pedaço de memória que foi alocado para poder liberar depois com free(). Se ao invés de fazer malloc 100 milhões de vezes eu fizer uma só e fizer 100 milhões de memset() o programinha fica 10 vezes mais rápido.

Isso significa que nós devemos evitar qualquer alocação que seja possível evitar? Depende. Pra começo de conversa esse é um teste extremo, não uma carga de trabalho real. Testes são sempre melhores em cargas reais (ou mais parecidas com algo real). Mas principalmente, se for pra piorar muito a legibilidade do código, torná-lo mais complexo, manter memória comprometida por mais tempo que o necessário, é importante que haja um ganho em desempenho para contrabalancear. E esse ganho tem que ser medido, não imaginado =).

 [1]: http://blog.kov.eti.br/?p=163