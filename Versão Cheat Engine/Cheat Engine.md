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
    KillNearestMobByName("Rat",20)--Mata 5 Ratos

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