
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
