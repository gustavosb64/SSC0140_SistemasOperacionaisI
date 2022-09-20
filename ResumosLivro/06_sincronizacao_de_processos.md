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
* Assumimos que todos os processos estejam executando a uma velocidade diferente de 0, mas não podemos fazer suposições com relação à **velocidade relativa** dos processos.
* É responsabilidade dos desenvolvedores do kernel assegurar que o SO esteja livre de condições de corrida.
    * **Kernel com preempção**
        * permite que um processo seja interceptado enquanto está sendo executado na modalidade de kernel.
        * particularmente difíceis de projetar para arquiteturas SMP.
        * mais apropriado para programação de tempo real.
    * **Kernel sem preempção**
        * NÃO permite que um processo seja interceptado enquanto está sendo executado na modalidade de kernel.
        * essencialmente livre de condições de corrida.

## 6.5 Mutex Locks (10<sup>th</sup> edition)
* **Mutex lock**:
    * protects critical sections and thus prevents race conditions.
    * a process musta acquire a lock before entering a critical section; it releases the lock when it exits.
* It has a boolena variable ```avaiable```.
* It can be either **contended** or **uncontended**:
    * **contended** if a thread blocks while trying to acquire the lock.
    * **uncontended** if it is available.
* **Main disadvantage**:
    * requires **busy waiting**.
* The type of mutex lock we have been describing is called **spin-lock** because it "spins" while waiting for the lock to become available.
 
## 6.6 Semaphores (10<sup>th</sup> edition)
* More robust than mutex lock, but works similarly.
* A **Semaphore** S is an int value that, apart from initialization, is accessesd only through two standard atomic operations: ```wait()``` and ```signal()```.
### 6.6.1 Semaphore Usage
* OSs usually distinguish between _counting_ and _binary_ semaphores.
    * **counting**:
        * can range over an unrestricted domain.
    * **binary**:
        * range only between 0 and 1.
        * behave similarly to mutex locks.
* Counting semaphores can be usesd to control access to a given resource consisting of a finite number of instances.
* Can be used to solve various synchronization problems.
    * Suppose we have process $P_1$ with a statement $S_1$ and a process $P_2$ with a statement $S_2$.
    * We require that $S_2$ be executed only after $S_1$ is completed.
    * Possible solution:
        * We can implement this scheme readily by letting $P_1$ and $P_2$ share a common semaphore synch, initialized in 0.
    * In $P_1$ and $P_2$ we can have, respectively:
```
S_1;
signal(synch)
```
```
wait(synch)
S_2;
```
### 6.6.2 Semaphore Implementation
* In order to avoid **busy waiting**, we can modify the definition of ```wait()``` and ```signal()```:
    * after executing ```wait()```, the process suspend itself, and it is put in a **wating queue**. 
    * The process is restarted by a ```wakeup()``` operation, and it is put in the ready queue.
```
    typedef struct{
        int value;
        struct process *list;
    }semaphore;
    
    wait(semaphore *S){
        S->value--;
        if (S->value < 0){
            add_this_process_to S->list;
            sleep();
        }
    }
    
    signal(semaphore *S){
        S->value++;
        if (S->value <= 0){
            remove_a_process_P_from S->list;
            wakeup(P);
        }
    }
```
* The ```sleep``` operation suspends the process that invokes it. The ```wakeup(P)``` operation resumes the execution of a suspended process P.
* **Note**: in this definition, semaphores values can be negative.
* The semaphore's list can use any queueing strategy; correct usage of semaphores does not depend on a particular one.
* **It is critical that semaphore operations be executed atomically**.
* We still haven't totally removed busy wating in this implementation.
 
### 6.9 Evaluation
* The hardware solutions for the critical-section problem presented so far are very low level.
* There has been a recent focus on using the CAS instructions to construct **lock-free algorithms** that provide protection from race conditions.
* These algorithms are often difficult to develop and test.
* CAS-based approaches are considered **optimistic** approaches.
* Mutual-exclusion locking approaches are considered **pessimistic** strategies.
* General rules concerning performance differences between CAS-based synchronization and traditional synchronizations:
    * **Uncontended**: CAS protection **will** be somewhat faster.
    * **Moderate contention**: CAS protection **will** be faster - possibly much faster.
    * **High contention**: under very highly contended loads, traditional synchronization will ultimately be faster.
* In _moderate contention_ scenario, the CAS operation succeeds most of the time, and when it fails, will iterate through a loop only a few times.
* The choice of a mechanism that addresses race conditions can also greatly affect system performance.
    * atomic integers are much lighter weight than traditional locks, and are generally more appropriate than mutex locks or semaphores sor single updates to shared variables such as counters.
