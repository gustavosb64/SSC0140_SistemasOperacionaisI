\newpage
# Capítulo 06: Sincronização de Processos

* _Processo cooperativo_: processos que podem ser afetados ou afetar outros processos. 

## 6.1 Antecedentes
* **Condição de corrida:** vários processos utilizam acessam os recursos da CPU concorrentemente e o resultado final depende da ordem de execução das tarefas.

## 6.2 O problema da Seção Crítica
* **Seção crítica:** seção de código de um processo em que ele pode estar alterando variáveis comuns, alterando uma tabela, escrevendo em um arquivo, etc.
* Quando um processo está em sua sessão crítica, nenhum outro processo pode.
* Para entrar em sua sessão crítica, cada processo deve solicitar permissão. A parte do código que realiza essa solicitação é a **seção de entrada**.
* A seção crítica pode ser seguida por uma **sessão de saída**. O que resta é a **seção remanescente**.
* Uma solução para o problema da seção crítica deve satisfazer:
    * **Exclusão mútua:**
        * Enquanto o processo $P_i$ estiver exectuando sua seção crítica, outros processos não poderão fazer o mesmo.
    * **Progresso:**
        * Se nenhum processo estiver exectuando sua seção crítica e alguns processos quiserem entrar em sua seções críticas, só os processos que não estiverem executando a seção remanescente participam da decisão.
    * **Espera limitada:**
        * Há um limite para o número de vezes em que outros processos podem entrar em suas respectivas seções críticas após um processo ter feito uma solicitação antes dessa solicitação ser atendida.
