# tiny-OS

lugar: representa o estado do sistema e é representada por um círculo;
ficha: indica que a condição associada ao lugar é verificada, representada po um ponto;
transição a ocorrência de um evento (transição de estado)
quantidade de recursos: representados por um peso no arco


o estado do sistema é dado pela distribuição das fichas nos lugares da rede de petri

a ocrrencia de um evento dispara uma transição na rede e assim muda o estado do sistema

A representação formal: R = <P, T, Pre, Pos>

* P é um conjunto finito de lugares pi;
* T é um conjunto finito de transições tj;
* Pre: PxT -> N (lugares precedentes de uma transição);
* Pos: PxT -> N (lugares seguintes a uma transição);

∧ = AND
∨ = OR
¬ = not
## Computation Tree Logic (CTL)

CTL is a logic for reasoning about properties of state-transition graphs. It can succintly express many properties of sequential circuits and communication protocols. 

Each operator of the logic has two parts: 
- Path quantifier: 
	- A—“for every path” 
	- E—“there exists a path”
- State quantifier: 
	- Fp—p holds sometime in the future 
	- Gp—p holds globally in the future 
	- Xp—p holds next time 
	- pUq—p holds until q holds

### Typical CTL formulas

* EF(started ∧ ¬ready): it is possible to get to a state where started holds but ready does not hold.
* AG(req ⇒ AF ack): if a request occurs, then it will be eventually acknowledged. 
* AG(AF device enabled): enabled holds infinitely often on every computation path. 
* AG(EF restart): from any state it is possible to get to the restart state.


### Liveness

Liveness properties express the assurance that something good will happen in a system. They are concerned with ensuring ongoing progress and the occurrence of desirable events over time.

1. **"Event aaa will occur eventually":** This statement is an example of a liveness property. It asserts that a particular event, denoted as "aaa," will happen at some point in the future. In the context of sequential programs, this could relate to the termination of the program, indicating that it will eventually finish executing.
    
2. **"Event aaa will occur infinitely many times":** This liveness property ensures that the event "aaa" will happen repeatedly and indefinitely. For example, in the context of dining philosophers, it could represent the guarantee that each philosopher will have an opportunity to dine (enter their critical section) an infinite number of times, indicating starvation freedom.
    
3. **"Whenever event bbb occurs, then event aaa will occur sometimes in the future":** This statement captures a cause-and-effect relationship between two events, "bbb" and "aaa." It expresses that if event "bbb" happens, event "aaa" will eventually occur in the future. In the context of processes waiting to enter their critical sections, this could mean that if a process is waiting ("bbb"), it will eventually enter its critical section ("aaa").



`AG (EF (tinyos.T_endLoading or tinyos.T_endUnload or tinyos.T_freeMemory or tinyos.T_startFirst or tinyos.T_startLoading or tinyos.T_startNext or tinyos.T_startUnload or tinyos.T_suspend))` checks if there is a liveness property in your system, ensuring that at least one of the specified transitions (events) eventually occurs in any reachable state. This formula states: For all states (AG), there exists a state where eventually (EF) at least one of the transitions (T) will occur+

### Deadlock check

In a deadlock, the system reaches a state where no further progress is possible.
You can check for deadlock by verifying that there exists a state from which no state transition is possible. 

- `EF` stands for "Eventually in the Future": It means that at some point in the future, the following condition should hold.
- `!` represents negation.
- `EX` stands for "Exists Next": It means there exists a next state where the condition holds.
- `True` represents the condition that is always true.

So, the formula `EF !(EX True)` essentially means that eventually, in the future, there exists a state from which there is no next state transition, which is a characteristic of a deadlock.


`AG(EF(deadlock))`: Esta fórmula está verificando duas coisas:

- `EF(deadlock)`: Ela verifica se em algum ponto no futuro (`EF`, "eventually in the future") é possível alcançar um estado em que a fórmula `deadlock` seja verdadeira. Isso significa que verifica se é possível chegar a um estado de deadlock em algum momento.
- `AG`: Depois de verificar a primeira parte, ela também verifica se a propriedade `EF(deadlock)` é verdadeira em todos os estados ao longo de todos os caminhos (`AG`, "globally for all paths"). Isso significa que não apenas verifica se é possível alcançar um estado de deadlock em algum momento, mas também se essa propriedade é verdadeira em todos os estados e caminhos possíveis.
