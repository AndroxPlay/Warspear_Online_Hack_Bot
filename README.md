# Warspear Online Bots List
<div align="center">
  Olá a todos, e sejam Muito Bem Vindos!!!
<br>Não Esqueça de dar uma passadinha no meu canal:
<br>
<br></div>

<div align="center">
  <a href="https://www.youtube.com/@androxplay822">
    <img src="https://github.com/user-attachments/assets/d84fcbe8-9829-4f3c-b68a-9f16db2f88e4" alt="Logo do AndroxPlay" />
  </a>
</div>

<div align="center">
  <a href="https://www.youtube.com/@androxplay822?sub_confirmation=1">
    <img src="https://github.com/user-attachments/assets/669d2a85-a98c-4845-ac71-4d7a139d4efe" alt="Subscribe!" />
  </a>
</div>

<br>
<div align="center">
Antes de começar, dê uma olhadinha em algo mais simples e básico:
  
[Player Information in C++.md](https://github.com/AndroxPlay/Warspear_Online_Hack_Bot/blob/10e74d637d39727630c95b2d209e1bfbb46f7a68/Warspear%20Leitor%20de%20Vida%20em%20C%2B%2B/Player%20Information%20in%20C%2B%2B.md)

Ou

[Player Information in Python.md](https://github.com/AndroxPlay/Warspear_Online_Hack_Bot/blob/a851b5d541a0009e698b51d86cf97b821830d8fd/Warspear%20Leitor%20de%20Vida%20em%20Python/Player%20Information%20in%20Python.md)
</div>




<br>Aqui Estão alguns dos Bots que trabalharemos para criar juntos:

## Entity Finder
![clideo_editor_17faabf9d8454385a14233d847783db6](https://github.com/user-attachments/assets/c1df88b3-6fc3-47e8-9fd9-2bcabcc5eedb)

O **Entity Finder** é uma automação que tem como propósito Encontrar Todas(ou uma Específica) entidade no sítio atual.

As entidades do Warspear giram em torno de uma **Entity_Tree**, porém a árvore em questão é bem diferente da normal...sendo uma Árvore-Rubro-Negra(**Red_Black_Tree**)...Essa Árvore-Rubro-Negra torna a pesquisa e a padronização de busca por entidades muitas vezes difícil, porém para superar essa limitação, podemos usar a pesquisa por AOB(**Array_Of_Bytes**).
<br>Por Exemplo
cada Conjunto de bytes representa uma Letra: 

![Fada](https://github.com/user-attachments/assets/16c84808-64d5-4c9f-a591-b6c13f8478da)
<br>Fada = "46 00 61 00 64 00 61 00"


```
 F(46 00)
 a(61 00)
 d(64 00)
 a(61 00)
```
![Rato](https://github.com/user-attachments/assets/2fd442b1-a687-4007-95af-789e91ba820c)
<br>Rato = "52 00 61 00 74 00 6F 00"

```
 R(52 00)
 a(61 00)
 t(74 00)
 o(6F 00)
```
![Cobra](https://github.com/user-attachments/assets/a2faee81-4603-40be-84cb-69513ca9fa48)
<br>Cobra = "43 00 6F 00 62 00 72 00 61 00"

```
 C(43 00)
 o(6F 00)
 b(62 00)
 r(72 00)
 a(61 00)
```
Porém a pesquisa pelo Nome do Mob não é o suficiente, visto que algumas vezes o mob pode aparecer mais de uma vez na memória(Morto, Referencia, Leitura de Sprites etc...)o recomendado nesses casos é utilizar o Nome do Mob+Bytes de Vida e Mana!

### Árvore Rubro Negra:
![wiki](https://github.com/user-attachments/assets/8c370684-bd67-4e46-b7b4-d1f248c6cc58)

https://pt.wikipedia.org/wiki/%C3%81rvore_rubro-negra


>Entity_Tree(Árvore de Entidades que aparece e desaparece ao spawnar ou morrer, oque gera um array com ids unicos para cada mob diferente)
><br><br>Array_Of_Bytes(Um Conjunto de Bytes que aparecem em padrão na memoria ram)



## Auto Targeter
![clideo_editor_bc5a7133423e400f9bf4715e81cd2533 (1)](https://github.com/user-attachments/assets/0bd31ac8-d76a-420e-a803-a96394d001e3)

O **Auto_Targeter** é uma automação que tem como propósito iniciar uma batalha entre o seu personagem e outra entidade.

Ao Criar o **Auto_Targeter** a maioria das pessoas normalmente usa algum tipo de **Image_Finder**, porém esse é um metodo errôneo, pois alguns mobs podem estar atrás de um outro player ou de uma árvore...oque causa uma falta de precisão e muitas vezes o bot não consegue encontrar a vítima...

>Image_Finder(Salvar uma imagem de um mob e procurar algo semelhante na tela usando script)

<br>Existem 2 formas comuns de criar o **Auto_Targeter**:

- 1 - Criar uma DLL que rode internamente no Processo,que faça uma checagem da entidade desejada na área e que chame a função ou mova o Ponteiro/Cursor até a entidade desejada! 
  Linguagens Úteis: (C++,C#,Python etc...)
  
- 2 - Criar um CT no Cheat Engine, que faça a checagem de entidade desejada na área e que mova o Ponteiro/Cursor até a entidade desejada! Linguagens Úteis: (Lua)



<br>Ao criar o **Auto_Targeter** precisamos das seguintes informações:

<br>Informações sobre o Player:
  
 - Posição (X/Y)
  
 - Quantidade de Vida (checagem de morte)
  
<br>Informações sobre o Cursor:

 - Posição(X/Y)
  
 - Status do Cursor:
  
    1- X: retorna não Atacável
    
    2- Espada: retorna Atacavel
    

<br>Informações sobre a Entidade:
 - Posição(X/Y)
 - Quantidade de Vida(checagem de morte)
 - Nome da Entidade:
 - White List: Mobs que o bot deve atacar
 - Black List: Mobs que o bot deve Ignorar
  Distância entre a Entidade até o Player
  Buffs e Debuffs
  

## Auto Attack
Em Desenvolvimento...
## Auto Loot
Em Desenvolvimento...
## Auto Heal
Em Desenvolvimento...
## Auto Quest
Em Desenvolvimento...
## Auto Follow
Em Desenvolvimento...
## Auto Buy/Sell
Em Desenvolvimento...
## Auto Craft
Em Desenvolvimento...
