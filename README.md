# Tiny OS

## Redes de Petri
- **Lugar:** Representa o estado do sistema e é representado por um círculo.
- **Ficha:** Indica que a condição associada ao lugar é verificada, representada por um ponto.
- **Transição:** Representa a ocorrência de um evento (transição de estado).
- **Quantidade de Recursos:** Representada por um peso no arco.

O estado do sistema é determinado pela distribuição das fichas nos lugares da rede de Petri.

A ocorrência de um evento dispara uma transição na rede, alterando assim o estado do sistema.

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

As propriedades de "Liveness" expressam a garantia de que algo bom acontecerá em um sistema. Elas estão preocupadas em garantir progresso contínuo e a ocorrência de eventos desejáveis ao longo do tempo.

1. **"O evento aaa ocorrerá eventualmente":** Esta afirmação é um exemplo de uma propriedade de "Liveness". Ela afirma que um evento específico, representado como "aaa", acontecerá em algum momento no futuro. No contexto de programas sequenciais, isso pode estar relacionado com a finalização do programa, indicando que ele eventualmente terminará de executar.
    
2. **"O evento aaa ocorrerá infinitamente muitas vezes":** Esta propriedade de "Liveness" assegura que o evento "aaa" acontecerá repetidamente e indefinidamente. Por exemplo, no contexto dos filósofos que jantam, isso poderia representar a garantia de que cada filósofo terá a oportunidade de jantar (entrar em sua seção crítica) um número infinito de vezes, indicando liberdade de inanição.
    
3. **"Sempre que o evento bbb ocorrer, então o evento aaa ocorrerá em algum momento no futuro":** Esta afirmação captura uma relação de causa e efeito entre dois eventos, "bbb" e "aaa". Ela expressa que se o evento "bbb" acontecer, o evento "aaa" ocorrerá eventualmente no futuro. No contexto de processos esperando para entrar em suas seções críticas, isso poderia significar que se um processo estiver esperando ("bbb"), ele eventualmente entrará em sua seção crítica ("aaa").
    

#### 1. Todas transições ocorrem em algum estado

```
AG (EF (tinyos.T_endLoading or tinyos.T_endUnload or tinyos.T_freeMemory or tinyos.T_startFirst or tinyos.T_startLoading or tinyos.T_startNext or tinyos.T_startUnload or tinyos.T_suspend))
``` 

Verifica que pelo menos uma das transições especificadas (eventos) eventualmente ocorrerá em qualquer estado alcançável. Esta fórmula afirma: Para todos os estados (AG), existe um estado em que eventualmente (EF) pelo menos uma das transições (T) ocorrerá.


Resultado: Verdadeiro

#### 2. Todas tasks finalizam eventualmente

```
AG (EF (tinyos.T_startLoading and tinyos.T_endUnload))
```

Verifica se em todos os estados, o sistema eventualmente chegara em um estado em que tanto a transição "startLoading" quanto "endUnload" ocorrem. Todas as tasks são completadas eventualmente?

Resultado: Verdadeiro
#### Tasks são executadas indefinidamente

```
AG (EF tinyos.T_startFirst)
```

A query verifica que as tasks são executadas Indefinidamente validando a execução da transição "startFirst" que representa o inicio das tasks;

Resultado: Verdadeiro
#### CPU Utilization

```
AG (EF tinyos.T_startNext)
```

Verifica que a transição "startNext" ocorre eventualmente em todos os estados, garantindo a utilização da CPU.

Resultado: Verdadeiro
#### Tasks entram no estado de "prontas"

```
AG (EF tinyos.P_TaskReady > 0)
```

Verifica que as tasks entram no estado de "pronto" eventualmente em todos os estados

Resultado: Verdadeiro
### Deadlock check

#### Método 01

```
AG(EF(deadlock)
```
Esta fórmula está verificando duas coisas:

- `EF(deadlock)`: Ela verifica se em algum ponto no futuro (`EF`, "eventually in the future") é possível alcançar um estado em que a fórmula `deadlock` seja verdadeira. Isso significa que verifica se é possível chegar a um estado de deadlock em algum momento.
- `AG`: Depois de verificar a primeira parte, ela também verifica se a propriedade `EF(deadlock)` é verdadeira em todos os estados ao longo de todos os caminhos (`AG`, "globally for all paths"). Isso significa que não apenas verifica se é possível alcançar um estado de deadlock em algum momento, mas também se essa propriedade é verdadeira em todos os estados e caminhos possíveis.

Resultado: Falso
#### Método 02

Em um deadlock, o sistema alcança um estado no qual nenhum progresso adicional é possível. Você pode verificar um deadlock ao verificar se existe um estado a partir do qual nenhuma transição de estado é possível.

- `EF` significa "Eventualmente no Futuro": Isso significa que em algum ponto no futuro, a seguinte condição deve ser verdadeira.
- `!` representa a negação.
- `EX` significa "Existe Próximo": Isso significa que existe um próximo estado no qual a condição é verdadeira.
- `True` representa a condição que é sempre verdadeira.

Portanto, a fórmula `EF !(EX True)` essencialmente significa que eventualmente, no futuro, existe um estado a partir do qual não há próxima transição de estado, o que é uma característica de um deadlock.

Resultado: Falso


`
