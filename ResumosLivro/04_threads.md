\newpage
# Capítulo 04: Threads

* A maioria dos sistemas operacionais atuais já fornece recursos que permitem que um processo contenha vários threads de controle.

## 4.1 Visão geral
* **Thread:** 
    * Unidade básica de utilização de CPU 
    * Composto por:
        * um ID de thread
        * um contador de programa
        * um conjunto de registradores
        * uma pilha
    * Compartilha com outros threads pertencentes ao mesmo processo: 
        * sua seção de código
        * a seção de dados
        * outros recursos do SO, como arquivos abertos e sinais
* **Processo pesado:**
    * Um processo tradicional, com **um único thread de controle.

### 4.1.1 Motivação
* Normalmente, uma aplicação é implementada como um processo separado com vários threads de controle.
* Exemplos:
    * Navegador Web:
        * um thread para exibir imageons ou texto e outro para recuperar dados da rede.
    * Servidor Web:
        * um thread para requisição de dados de uma página
        * se não fosse assim, poderia atender a apenas um cliente pro vez.
* A maioria dos kernels dos SOs já são multithread.
 
### 4.1.2 Benefícios 
* Os benefícios da programção com vários threads podem ser dividos em 4 categorias principais:
    * **Capacidade de resposta:** permite que um programa continue a ser executado mesmo se parte dele estiver executando uma tarefa demorada.
    * **Compartilhamento de recursos:** por default, os threads compartilham a memória e os recursos do processo ao qual pertencem.
    * **Economia:** alocação de memória e recursos para processos é dispendiosa; é mais econômico criar threads e variar apenas o contexto.
    * **Escalabilidade:** os benefícios do uso de vários threads podem ser muito maiores em uma arquitetura com mútios processadores.
 
### 4.1.3 Programação Multicore
* Programação com threads fornece um mecanismo para o uso mais eficiente de muitos núcleos de processador e o aumento da concorrência.
* Em um sistema com vários núcleos, vários threads podem ser executados paralelamente.
* 5 áreas apresentam desafios na programação para sistemas multicore:
    * **Divisão de atividades:** análise das aplicações em busca de áreas qeu possam ser divididas em tarefas concorrentes.
    * **Equilíbrio:** as tarefas devem ser executadas com esforço de mesmo valor (se uma tarefa não contribui muito para o processo, reservar um núcleo só pra ela não vale a pena).
    * **Divisão de dados:** os dados acessados e manipulados pelas tarefas devem ser dividos em núcleos separados.
    * **Dependência de dados:** deve ser analisada a depêndencia entre diferentes tarefas para garantir a sincronização.
    * **Teste e depuração.** 

## 4.2 Modelos de Geração de Multithread
