# O início:
![Static Badge](https://img.shields.io/badge/require-c%2B%2B-%20?style=flat&logo=windows&color=red)
<br>Antes de começar a se aventurar procurando mobs para matar, é recomendado treinar e aprender o basico!
É recomendado que você saiba o básico de uma estrutura em C++:
    
    #include <windows.h>
    #include <iostream>
    
    using namespace std;
    
    int main()
    {
    	cout << "Eu sou um Texto :D" << endl;
    	cout << "Digite um Numero: " << endl;
    	int numero = 0;
    	cin >> numero;
    	cout << "O valor que voce digitou foi: " << numero << endl;
    	return 0;
    };

Também é recomendado que você aprenda sobre a API do windows, pois vamos usar algumas funções muito conhecidas:
<br>

**FindWindowA**
<br>
Para encontrar-mos a janela pelo nome (Warspear Online) usare-mos o [FindWindow:](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-findwindowa)

    HWND FindWindowA(
        LPCSTR lpClassName,
        LPCSTR lpWindowName
    );

Ex:

    HWND warspear_window = FindWindow(NULL, "Warspear Online");
Essa função nos retorna um identificador de janela HWND!<br><br>

**GetWindowThreadProcessId**
<br>
Com um identificador de janela podemos prosseguir para pegar o ID de processo usando [GetWindowThreadProcessId:](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getwindowthreadprocessid)

    DWORD GetWindowThreadProcessId(
        HWND    hWnd,
        LPDWORD lpdwProcessId
    );
Ex:


    DWORD process_id = 0;
    GetWindowThreadProcessId(warspear_window, &process_id);

Usamos o **(&)** para salvar o valor do id dentro da variavel process_id!
Vale notar que esse valor é uma variável do tipo Dword ao qual usaremos no próximo passo!<br><br>

**OpenProcess**
<br>
Com o ID do processo podemos abrir ele com propriedades de escrita e leitura de dados, e para isso usamos o [OpenProcess:](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess)

    HANDLE OpenProcess(
        DWORD dwDesiredAccess,
        BOOL  bInheritHandle,
        DWORD dwProcessId
    );
Ex:

    HANDLE warspear_process = OpenProcess(PROCESS_ALL_ACCESS, true, process_id);

Precisávamos de um handle para utilizarmos no próximo passo que é finalmente o de leitura de memória!<br><br>

**ReadProcessMemory**
<br>
Finalmente podemos ler a memória do nosso game!
Precisamos apenas preencher as informações que a função [ReadProcessMemory](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-readprocessmemory) nos pede:

    BOOL ReadProcessMemory(
        HANDLE  hProcess,
        LPCVOID lpBaseAddress,
        LPVOID  lpBuffer,
        SIZE_T  nSize,
        SIZE_T  *lpNumberOfBytesRead
    );

Ex:

    ReadProcessMemory(warspear_process, (DWORD*)0x01234ABC, &valor_lido, 4, &bytes_read);

Essa função nos retorna o valor que queremos saber dentro da variável:

    valor_lido

Tambem retorna a quantidade de bytes lidos dentro da variável:

    bytes_read

E ela requer 2 parâmetros que precisamos indicar, o primeiro sendo o Endereço de memória que queremos ler e o tipo dele, nesse caso um DWORD* para identificar que é um DWORD com ponteiro para um valor:

    (DWORD*)0x1234ABC
E em seguida o valor tipo de valor de bytes que desejamos ler:

    4

Oque nos leva ao seguinte código:

    #include <windows.h>
    #include <iostream>
    
    using namespace std;
    
    int main()
    {
    	int vida = 0;
    	DWORD valor_lido = 0;
    	DWORD bytes_read = 0;
    
    	HWND warspear_window = FindWindowA(NULL,"Warspear Online");
    
    	DWORD process_id = 0;
    	GetWindowThreadProcessId(warspear_window, &process_id);
    
    	HANDLE warspear_process = OpenProcess(PROCESS_ALL_ACCESS, true, process_id);
    
    	ReadProcessMemory(warspear_process, (DWORD*)0x0EB951E8, &valor_lido, 4, &bytes_read);
    	
    	vida = valor_lido;
    
    	cout << vida << endl;
    
    	return 0;
    };
Resultado:<br>
![result](https://github.com/user-attachments/assets/240ab3c3-8609-427d-93e0-a5ae53147367)


Lembrando que o endereço: 0x0EB951E8 é dinâmico e não estático, visto que o game busca sempre poupar memória...e para isso vamos ler a memória e somar o deslocamento de ponteiro:

![image](https://github.com/user-attachments/assets/af08e948-7433-4235-aca5-fb131180d1a7)

Nesse caso **"warspear.exe" = 0x400000** e representa o endereço base do nosso executável, e logo em seguida temos **0x847FF8** que seria o deslocamento do game na memória ram, a soma desses 2 valores resulta em
**0xC47FF8** que é um novo endereço, se formos ler a memória dentro desse endereço vamos obter um novo endereço: **0x0FC07644 
(Nota: esse endereço muda sempre que você abre o jogo, visto que o game sempre vai abrir em um local diferente da memória...porém a soma continua a mesma, oq muda é apenas para onde a soma aponta!)**

Então usaremos uma variável do tipo uintptr_t para criar um ponteiro para o endereço base:

    uintptr_t base = 0x400000;
E para o endereço de deslocamento na memória:

    uintptr_t desloc_memo = 0x847FF8;
E em seguida vamos somar esses 2 valores e guardá-los em uma variável do mesmo tipo e ler a memória para ver para onde o resultado dessa soma está apontado:

    uintptr_t total = base + desloc_memo;
    ReadProcessMemory(handle, (DWORD*)total, &total, 4, &bytes_read);
	cout << "Valor da Soma Base " << total << endl;

**Offsets**

![image](https://github.com/user-attachments/assets/134752c7-113a-4b8d-b1a7-e7708b9667bd)

Agora existem 2 formas de prosseguir, você pode criar um loop que leia a memória e some um offset de cada vez, ou pode somar aos poucos e verificando se o resultado bate, e é oq faremos a seguir:

De cara vamos somar o offset **0** o qual não mudará nada kkkk, mas é importante manter o padrão:

    total = total + 0;

E em seguida faremos uma nova leitura:

    ReadProcessMemory(warspear_process, (DWORD*)total, &total, 4, &bytes_read);
