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
Para encontrar-mos a janela pelo nome (Warspear Online) usare-mos o [FindWindow:](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-findwindowa)

    HWND FindWindowA(
        LPCSTR lpClassName,
        LPCSTR lpWindowName
    );

Ex:

    HWND warspear_window = FindWindow(NULL, "Warspear Online");
Essa função nos retorna um identificador de janela HWND!<br><br>

**GetWindowThreadProcessId**
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
