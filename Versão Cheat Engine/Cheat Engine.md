# O Básico:

<br>Antes de começar a construir e editar o projeto, precisamos definir quais informações precisamos para montar o nosso bot, por exemplo:

Auto Target:

Precisamos de uma lista de mobs para selecionar um para atacar(Entity Finder)
O Player está vivo?
Qual Nome do Mob?
Qual a Posição do Mob - X / Y?
O Mob está vivo?
Qual a Posição do Cursor - X / Y?
Qual a Flag do Cursor?
    

Com essas informações, podemos construir algumas funções que usam essas informações, Ex:

    --Auto Target:

    KillNearestMobByName("Rat")
    --ou
    KillMob("Rat")

Assim o codigo pode matar a entidade com base no nome, e tambem, podemos passar parâmetros. Ex:

    --Auto Target:

    KillMob("Rat", 5) --Mata 5 Ratos
    --ou
    KillNearestMobByName("Rat",20)--Mata 20 Ratos

O Auto Loot funciona da mesma forma, utilizando o mesmo processo, porém, devemos lembrar que só existe Loot após matarmos algum mob, então uma abordagem mais inteligente seria:

    --Auto Loot
    CollectLoot("Rato", "Gota de veneno", 5) --Mata Ratos até pegar 5 gotas de veneno.
                                             --Precisamos conferir o drop pelo nome, e
                                             --de um contador para confirmar que foi
                                             --dropado.
    --ou
    CollectItemsFromMob("Mountain Goat", "Mountain Goat's Heart", 5)

Para Quest(Plantas, Npcs):

    --Auto Quest
    CollectPlants("Evilflower", "Evilflower", 5)

    --Auto Talk
    TalkToNPC("Kaaf", 0) --0 representa a primeira missão, 1 a segunda missão, 2 a terceira missão...
                         --A função pode clicar no npc e apertar F2 para aceitar(Somente quando abrir
                         --a tela de aceitar a missão)


Lembrando, para que tudo isso seja possível, precisamos identificar tudo que contém no sítio atual, para isso, precisamos pesquisar as entidades:

    KillMob("Rat", 5)
        --Por debaixo dos Panos:

        --Cria contador de mobs mortos
        --Cria armazenador de ids de foco para ataque

        --Deve Retornar uma Lista de Mobs na área
        --Nome - X, Y, Id
        --Rat - 12, 6, 513123982
        --Rat - 17, 8, 513123984
        --Rat - 20, 15, 513123983
        --Rat - 20, 19, 513123981
        --Snake - 17, 24, 51312390
        --Snake - 25, 13, 513123988

        --Então ele deve escolher o mais próximo do player (Vamos precisar do X e Y do player)
        --Mover o cursor até a posição do mob(Para Test, comece com o manequim - Ele nao se move)
        --Checar a Flag do Mouse(**A Checar Valores de Flags** 17 - Ataque, 0 - Chão)
        --Pressionar Enter para atacar o mob
        --Entrar em Loop para checar se o mob morreu evitar que ataque outro utilizando trava por id
        --Morte do mob confirmada, encerramento do loop, Remoção da trava por Id do mob
        --Adiciona +1 ao Contador de Mobs Mortos
        --Obtém a lista de mobs novamente para atualizar(Caso algum outro player tenha matado algum mob)
