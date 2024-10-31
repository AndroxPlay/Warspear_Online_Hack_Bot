
# O início:
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
<br>Antes de começar a se aventurar procurando mobs para matar, é recomendado treinar e aprender o basico!
É recomendado que você saiba o básico de uma estrutura em Python:
    
    print("Eu sou um Texto :D")
    numero = 0
    numero = input("Digite um Numero: " )
    print("Você digitou: ", numero)




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
[Leitor de Vida Python(WarspearOnline](https://github.com/AndroxPlay/Warspear_Online_Hack_Bot/blob/02a4a13bebef5e4b22673946378a48180c861717/Warspear%20Leitor%20de%20Vida%20em%20Python/Leitor%20de%20Vida%20Python(WarspearOnline).exe)
