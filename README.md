
# TP1 – Mundo dos Blocos

## Equipe  
- **Disciplina**: Inteligência Artificial
**Professor:**: Edjard Mota
- **Curso**: Engenharia Da Computação

- **Integrantes**:  
  - Allan Carvalho de Aguiar – 22153696  
  - Ricardo Mendonça Braz – 22152017  
  - Luã Henrique Souza da Silva – 21651379  

---

## Apresentação do Projeto

Este trabalho consiste na criação de um sistema de planejamento em **Prolog**, inspirado no problema clássico do “Mundo dos Blocos”, conforme descrito no Capítulo 17 do livro *Prolog Programming for Artificial Intelligence*, de Ivan Bratko. A proposta é desenvolver um planejador baseado em ações, que manipula blocos através de regras lógicas para alcançar um estado final definido a partir de uma configuração inicial.

O sistema lida com blocos identificados pelas letras **a, b, c** e **d**, que podem ser movidos entre posições na mesa ou sobre outros blocos, conforme as condições do cenário e os objetivos estabelecidos.

---

## Visão Geral

O projeto estrutura um sistema automatizado de planejamento, onde blocos estão posicionados em diferentes locais e podem ser reorganizados. O sistema é capaz de:

- Planejar a movimentação dos blocos desde um estado inicial até uma configuração final;
- Realizar ações que envolvam a movimentação dos blocos, respeitando restrições como posições livres e condições prévias;
- Verificar se todos os objetivos foram atingidos, regredir metas e planejar com base nas ações possíveis.

### Princípios do Mundo dos Blocos

- **Estados**: Um estado representa a disposição atual dos blocos. Eles podem estar diretamente sobre a mesa (em coordenadas únicas) ou empilhados, ocupando múltiplas posições.
- **Ações**: As ações envolvem mover blocos, desde que estejam livres e que o destino esteja disponível, obedecendo às pré-condições.
- **Objetivos (Metas)**: O sistema visa alcançar uma configuração específica dos blocos, como colocá-los em determinadas posições ou empilhados de maneira específica.

---

## Estruturas Fundamentais

1. **Estado Inicial**: Representa a posição dos blocos no início da execução.
2. **Estado Objetivo**: Indica a posição esperada de cada bloco ao final da execução do plano.

---

## Funcionamento do Sistema

### Fluxo Operacional

1. **Identificação das Metas**: O sistema identifica as metas que precisam ser alcançadas.
2. **Seleção de Ações**: Para cada meta, busca-se uma ação que possa satisfazê-la, considerando suas pré-condições. Caso uma ação não seja viável, o sistema regrede e busca um novo caminho.
3. **Execução do Plano**: As ações são realizadas em sequência, movimentando os blocos até que o estado final seja alcançado.

### Exemplo de Execução

Estado inicial definido:

```prolog
initial_state([
  on(c, p([1,2])),          % c ocupa as posições 1 e 2 da mesa
  on(a, 4),                 % a está na posição 4
  on(b, 6),                 % b está na posição 6
  on(d, supports(a,b)),     % d está sobre os blocos a e b
  clear(c),                 
  clear(d),                 
  clear(3),                 
  clear(5)                  
 ]).
```

Estado final desejado:

```prolog
goal_state([
  on(a, c),                 % a deve ficar sobre c
  on(d, p([3,4,5])),        % d deve ocupar as posições 3 a 5
  on(b, 6),                 
  clear(a),
  clear(b),
  clear(d)
]).
```

O sistema processa essas informações e gera uma sequência de ações que transforma o estado inicial no estado final.

---

## Instruções para Execução

### Usando o SWISH (online)

1. Acesse o ambiente [SWISH](https://swish.swi-prolog.org/).
2. Certifique-se de manter os arquivos separados como `blocks_world_definitions.pl`, `blocks_world_actions.pl` e `blocks_world_planner.pl`, e que as linhas abaixo estejam presentes no início do `blocks_world_planner.pl`:
   ```prolog
   :- use_module(blocks_world_definitions).
   :- use_module(blocks_world_actions).
   ```
3. Defina os predicados `initial_state/1` e `goal_state/1`.
4. Execute o seguinte comando:
   ```prolog
   ?- initial_state(State), goal_state(Goals), plan(State, Goals, Plan).
   ```
5. Clique em **Run** para visualizar o plano gerado.

---

### Usando o SWI-Prolog (localmente)

1. Instale o SWI-Prolog via [site oficial](https://www.swi-prolog.org/Download.html).
2. Abra o terminal (Linux/macOS) ou o prompt de comando (Windows).
3. Navegue até o diretório onde seus arquivos `.pl` estão salvos.
4. Inicie o interpretador com:
   ```bash
   swipl
   ```
5. Carregue os arquivos na ordem:
   ```prolog
   ?- [blocks_world_definitions].
   ?- [blocks_world_actions].
   ?- [blocks_world_planner].
   ```
6. Execute o planejamento:
   ```prolog
   ?- initial_state(State), goal_state(Goals), plan(State, Goals, Plan).
   ```

Você também pode testar ações isoladas com comandos como:

```prolog
?- initial_state(S0), apply_action(move(c, p([1,2]), d), S0, S1).
?- initial_state(S0), possible(move(a, 4, c), S0).
```
