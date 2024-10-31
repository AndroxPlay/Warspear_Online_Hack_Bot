


# O início:
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
<br>Antes de começar a se aventurar procurando mobs para matar, é recomendado treinar e aprender o basico!
É recomendado que você saiba o básico de uma estrutura em Python:
    
    print("Eu sou um Texto :D")
    numero = 0
    numero = input("Digite um Numero: " )
    print("Você digitou: ", numero)

Para ler a memória do warspear precisamos utilizar a biblioteca `Pymem` que permite acessar a memória dos programas que rodam no Windows!

**Instalando Pymem**
<br>
Primeiro temos que abrir o cmd e digitar:

    pip install pymem
![pyt](https://github.com/user-attachments/assets/7ffd1a7e-afef-474e-92fc-55c5848419a2)

Em seguida podemos criar um arquivo que tenha a extensão .py para rodar o nosso programa...
![leitor de memopy](https://github.com/user-attachments/assets/7244d115-4af0-4a0a-8f6b-0adb9a21bc78)

Nele vamos importar a biblioteca que instalamos:

    import pymem
Em seguida vamos pegar o id do processo pelo nome do processo e vamos salvar esse id em uma variável:

    process_id = "warspear.exe"
    pm = pymem.Pymem(process_id)

Em seguida podemos criar uma variável que seja nosso endereço da memória com a vida do personagem:

    resultado = 0x00D30210
![pyt](https://github.com/user-attachments/assets/e080de91-8a65-42c8-912f-abca9487fe3c)

Finalmente podemos ler esse endereço usando **pm.read_int**:

    resultado = pm.read_int(resultado)
    print("HP: ",resultado)

Resultado:
<br>
![off](https://github.com/user-attachments/assets/2cc585d6-3bc1-45b9-91be-d0b2dc7c6848)
Lembrando que o endereço: 0x00D30210 é dinâmico e não estático, visto que o game busca sempre poupar memória...e para isso vamos ler a memória e somar o deslocamento de ponteiro:
![Capturar](https://github.com/user-attachments/assets/13f7712d-21b4-4c53-b4ec-d1e5f8fa3958)

Nesse caso **"warspear.exe" = 0x400000** e representa o endereço base do nosso executável, e logo em seguida temos **0x847FF8** que seria o deslocamento do game na memória ram, a soma desses 2 valores resulta em **0xC47FF8** que é um novo endereço, se formos ler a memória dentro desse endereço vamos obter um novo endereço: **0xE15DCEC
 (Nota: esse endereço muda sempre que você abre o jogo, visto que o game sempre vai abrir em um local diferente da memória...porém a soma continua a mesma, oq muda é apenas para onde a soma aponta!)**

Então voltaremos lá encima e criaremos 2 variáveis que servirão de endereço base e endereço de deslocamento de memória:

    base = 0x400000
    desloc_memo = 0x00847FF8

E em seguida vamos somar esses 2 valores e guardá-los em nossa variável e vamos também ler a memória (sobrescrevendo a variável criada: **resultado**) para ver para onde o resultado dessa soma está apontado:

    resultado = desloc_memo + base
    resultado = pm.read_int(resultado)
	print("Primeiro Offset",resultado )
  Resultado: <br>
![primeiro offset](https://github.com/user-attachments/assets/733ee890-0f7e-4ff0-b456-e22a1e0c6c7f)
**Offsets**

![image](https://github.com/user-attachments/assets/134752c7-113a-4b8d-b1a7-e7708b9667bd)

Agora existem 2 formas de prosseguir, você pode criar um loop que leia a memória e some um offset de cada vez, ou pode somar aos poucos e verificar se o resultado bate, e é oq faremos a seguir:

De cara vamos somar o offset **0** o qual não mudará nada kkkk, mas é importante manter o padrão:

    resultado = resultado + 0x0
    
E em seguida faremos uma nova leitura e salvaremos na mesma variável **resultado**:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 1 offset")

Resultado:
<br>
![primeiro offset](https://github.com/user-attachments/assets/2b4b4082-3863-44b1-a90c-d32f6a7179ec)
Assim podemos somar o próximo offset  **0x14**:


    resultado = resultado + 0x14


Novamente Faremos uma nova leitura:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 2 offset")
    
Resultado:
<br>
![primeiro offset](https://github.com/user-attachments/assets/5dce6817-9803-4f86-95e0-7b67b736e978)
E somar o próximo offset  **0x54**:


    resultado = resultado + 0x54


Faremos uma nova leitura:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 3 offset")

Resultado:
<br>

![primeiro offset](https://github.com/user-attachments/assets/da97a3d3-697b-499b-be51-cceb6a0aadf5)
E somar o próximo offset  **0x6C**:

    resultado = resultado + 0x6C


Faremos uma nova leitura:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 4 offset")
Resultado:
<br>
![segundo offset](https://github.com/user-attachments/assets/bfdab39c-2535-42a7-ac29-18cd28d8b4ae)
E somar o próximo offset  **0x280**:

    resultado = resultado + 0x280


Faremos uma nova leitura:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 5 offset")
Resultado:
<br>
![terceiro offset](https://github.com/user-attachments/assets/b6d51caf-576c-4ab5-92f0-131508fdc983)
Podemos notar que os offsets estão vindo exatamente iguais, e está tudo bem, podemos continuar e somar o próximo offset **0x3C0**:

    resultado = resultado + 0x3C0


Faremos uma nova leitura:

    resultado = pm.read_int(resultado)
    print(resultado, "resultado com 6 offset")
    
Resultado:
<br>
![terceiro offset](https://github.com/user-attachments/assets/88131791-d437-4757-8101-56e1f34db113)

<br>E finalmente **0x110-0x8** que é igual a **0x108**:
    
    resultado = resultado + 0x108


Faremos uma nova leitura:


    resultado = pm.read_int(resultado)
    print("HP: ",resultado)
    
Resultado:
<br>
![Hp](https://github.com/user-attachments/assets/a41912e5-f143-4500-813b-945be640fc9b)

Aí está o valor da vida do Personagem!

Caso Queira o Código completo:

    import pymem
    
    process_id = "warspear.exe"  # O ID do processo em hexadecimal
    pm = pymem.Pymem(process_id)
    
    address = 0x00847FF8
    address2 = 0x00400000
    
    resultado_addresses = address + address2
    resultado = pm.read_int(resultado_addresses)
    print(resultado, "resultado sem as offsets")
    
    resultado_offset = resultado + 0x0
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 1 offset")
    
    resultado_offset = resultado + 0x14
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 2 offset")
    
    resultado_offset = resultado + 0x54
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 3 offset")
    
    resultado_offset = resultado + 0x6C
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 4 offset")
    
    resultado_offset = resultado + 0x280
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 5 offset")
    
    resultado_offset = resultado + 0x3C0
    resultado = pm.read_int(resultado_offset)
    print(resultado, "resultado com 6 offset")
    
    resultado_offset = resultado + 0x108
    resultado = pm.read_int(resultado_offset)
    print("HP: ",resultado)
    
    input()

Lembrando que esse código é útil até a versão atual do 
**Warspear Online: 12.6.0 24/10/2024-1**

Executável para Testar em seu Computador:
<br>
[Leitor de Vida Python(WarspearOnline)](https://github.com/AndroxPlay/Warspear_Online_Hack_Bot/blob/02a4a13bebef5e4b22673946378a48180c861717/Warspear%20Leitor%20de%20Vida%20em%20Python/Leitor%20de%20Vida%20Python(WarspearOnline).exe)
