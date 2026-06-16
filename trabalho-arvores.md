# Trabalho – Estruturas Avançadas de Árvores

**Disciplina:** Estrutura de Dados  
**Curso:** Análise e Desenvolvimento de Sistemas  
**Integrantes:** Jean 

---

## Parte 1 – Tipos de Árvores

### AVL

#### Conceito

A Árvore AVL é uma Árvore Binária de Busca (BST) **auto-balanceável**, criada em 1962 pelos matemáticos soviéticos Georgy **A**delson-**V**elsky e Evgenii **L**andis — daí o nome AVL. Ela foi a primeira estrutura de dados a garantir, automaticamente, que a árvore permaneça equilibrada após qualquer inserção ou remoção.

O mecanismo central é o **fator de balanceamento (FB)**: para cada nó, a diferença entre a altura da subárvore esquerda e a da subárvore direita deve ser sempre **-1, 0 ou +1**. Se uma operação violar essa regra, a árvore realiza **rotações** para se reequilibrar imediatamente.

#### Características

- Todo nó armazena seu **fator de balanceamento** (FB = altura_esq − altura_dir)
- O FB de qualquer nó deve pertencer ao conjunto {−1, 0, +1}
- Toda AVL é uma BST válida (filho esquerdo < nó < filho direito)
- Rotações simples ou duplas são executadas automaticamente após desequilíbrio
- A altura de uma AVL com *n* nós é sempre **O(log n)**, garantida matematicamente

#### Vantagens

- **Busca eficiente e previsível:** garante O(log n) mesmo no pior caso, sem degeneração
- **Balanceamento rigoroso:** mantém a árvore na menor altura possível entre as estruturas balanceadas
- **Ideal para leituras frequentes:** sistemas onde consultas superam em muito as modificações se beneficiam diretamente

#### Desvantagens

- **Inserção e remoção mais custosas:** cada operação pode exigir múltiplas rotações em cascata
- **Overhead de memória:** cada nó precisa guardar o valor do fator de balanceamento
- **Implementação mais complexa** do que uma BST simples, especialmente o gerenciamento de rotações

#### Exemplo Ilustrado

**Inserção de 10 → 20 → 30 em uma AVL:**

Após as três inserções, o nó raiz (10) fica com FB = −2 (desbalanceado à direita). Uma **rotação simples à esquerda** é executada, promovendo o nó 20 à raiz:

```
Inserção: 10, 20, 30

Passo 1 – após inserir 10 e 20:   Passo 2 – após inserir 30 (desequilíbrio!):

    10 (FB=-1)                          10 (FB=-2) ← desequilibrado!
      \                                   \
      20 (FB=0)                           20 (FB=-1)
                                            \
                                            30 (FB=0)

Passo 3 – rotação simples à esquerda em 10:

         20 (FB=0)
        /         \
     10 (FB=0)   30 (FB=0)   ← árvore equilibrada ✓
```

---

### Rubro-Negra

#### Conceito

A Árvore Rubro-Negra (Red-Black Tree) é uma BST auto-balanceável em que cada nó recebe uma **cor** — **vermelho** ou **preto** — e um conjunto de regras de coloração garante que a árvore permaneça aproximadamente balanceada.

Diferente da AVL, ela não impõe balanceamento perfeito: o caminho mais longo da raiz até uma folha pode ser até **o dobro** do caminho mais curto. Em compensação, realiza **menos rotações** por operação, tornando inserções e remoções mais rápidas na prática.

#### Regras de Coloração

Toda Árvore Rubro-Negra válida obedece as cinco propriedades abaixo simultaneamente:

1. **Todo nó é vermelho ou preto.**
2. **A raiz é sempre preta.**
3. **Toda folha sentinela (NIL) é preta.**
4. **Se um nó é vermelho, ambos os seus filhos são pretos.** (Nunca há dois nós vermelhos consecutivos.)
5. **Para qualquer nó, todos os caminhos simples desse nó até suas folhas descendentes contêm o mesmo número de nós pretos.** (Black-height uniforme.)

As regras 4 e 5 juntas garantem que a árvore nunca fique muito desequilibrada: a black-height uniforme impede que um ramo cresça indefinidamente mais que os outros.

#### Vantagens

- **Menos rotações que a AVL:** operações de inserção e remoção são mais ágeis em média
- **Boa para escrita frequente:** o rebalanceamento por coloração é mais leve que o da AVL
- **Amplamente usada em implementações industriais:** `std::map`/`std::set` no C++, `TreeMap` no Java, e até o escalonador CFS do kernel Linux

#### Desvantagens

- **Busca levemente menos eficiente que AVL:** a árvore aceita ser um pouco mais "alta" que o estritamente necessário
- **Implementação mais complexa:** os numerosos casos de recoloração e rotação tornam o código extenso e difícil de depurar
- **Mais difícil de compreender conceitualmente** que a AVL para iniciantes

#### Exemplo Ilustrado

**Árvore Rubro-Negra com valores 10, 20, 30, 15, 25:**

```
              [20]  ← (P) Preto / Raiz
              /    \
          [10]     [30]       ← (P) Preto
            \       /
           [15]   [25]        ← (V) Vermelho
           
Legenda: (P) = Nó Preto   (V) = Nó Vermelho
Os ponteiros NIL não mostrados são todos pretos.
```

Verificação das propriedades:
- Raiz (20) é preta ✓
- Nós vermelhos (15 e 25) têm apenas filhos pretos (NIL) ✓
- Todo caminho da raiz até NIL passa por 2 nós pretos (black-height = 2) ✓

---

### N-ária

#### Conceito

Uma Árvore N-ária é uma estrutura em que cada nó pode ter até **N filhos**, onde N é um inteiro maior que 2. Enquanto a árvore binária restringe cada nó a no máximo 2 filhos, a N-ária generaliza essa ideia para qualquer número.

As variações mais importantes incluem:

- **Árvore B (B-Tree):** nós com até N filhos; usada em bancos de dados e sistemas de arquivos
- **Árvore B+ (B+Tree):** variação da B-Tree onde todos os dados ficam nas folhas, ligadas em lista encadeada
- **Trie (Árvore de Prefixos):** especializada em buscas por prefixo de strings; cada aresta representa um caractere
- **Árvore N-ária genérica:** usada para representar hierarquias como menus, categorias e taxonomias

#### Diferenças em Relação às Árvores Binárias

| Característica        | Árvore Binária               | Árvore N-ária                              |
|-----------------------|-----------------------------|--------------------------------------------|
| Filhos por nó         | No máximo 2                 | No máximo N (N > 2)                        |
| Altura da árvore      | Maior (mais níveis)         | Menor (menos níveis)                       |
| Acessos necessários   | Mais (por ser mais alta)    | Menos (cada nó "descarta" mais ramos)      |
| Uso de memória / nó   | Menor                       | Maior (mais ponteiros e chaves por nó)     |
| Aplicação típica      | Busca em memória, ordenação | Hierarquias, índices em disco, DBs         |

#### Vantagens

- **Menor altura:** com mais filhos por nó, a árvore cresce menos em profundidade — uma B-Tree de ordem 1000 com 3 níveis indexa até 1 bilhão de registros
- **Excelente para armazenamento em disco:** um nó inteiro cabe em um bloco de disco (4 KB ou 16 KB), minimizando operações de I/O
- **Representa hierarquias naturais** com múltiplos filhos por elemento de forma direta e intuitiva

#### Desvantagens

- **Maior uso de memória por nó:** cada nó armazena múltiplas chaves e múltiplos ponteiros
- **Algoritmos mais complexos:** busca, inserção (split) e remoção (merge) dentro dos nós exigem lógica adicional
- **Overhead de gerenciamento de chaves** dentro de cada nó adiciona complexidade de implementação

#### Exemplo Ilustrado

**Árvore B de ordem 3 (máx. 2 chaves e 3 filhos por nó):**

```
                  [ 30 | 60 ]
                 /     |     \
          [10|20]    [40|50]    [70|80]
```

Neste exemplo, a raiz contém 2 chaves (30 e 60) e 3 filhos. Cada filho também pode ter até 2 chaves e 3 filhos. A estrutura se mantém balanceada (todas as folhas no mesmo nível).

#### Aplicações Práticas

- **Sistemas de arquivos:** NTFS (Windows) e ext4/Btrfs (Linux) usam B-Trees para indexar metadados e localizar arquivos no disco
- **Bancos de dados relacionais:** MySQL (InnoDB), PostgreSQL e Oracle usam B+Trees como estrutura principal dos índices
- **DNS:** a hierarquia de domínios é uma árvore N-ária (raiz → `.com` → `google` → `www`)
- **Menus de aplicativos:** estruturas de menus com itens e submenus aninhados em múltiplos níveis
- **Compiladores:** Árvores de Sintaxe Abstrata (AST) possuem nós com número variável de filhos

---

## Parte 2 – Operações em Árvores

### Rotação Simples à Direita

#### Objetivo

A rotação simples à direita tem como objetivo **reequilibrar** um nó de uma Árvore AVL cujo fator de balanceamento indica que a **subárvore esquerda está excessivamente alta**. A operação "empurra" a raiz desequilibrada para baixo e à direita, elevando o filho esquerdo ao seu lugar.

#### Situação em que é Utilizada

É aplicada no **caso LL (Left-Left):** o desequilíbrio ocorre porque foi inserido um elemento na subárvore esquerda do filho esquerdo do nó desbalanceado.

Sinal de uso: o nó problemático possui **FB = −2** (subárvore esquerda 2 níveis mais alta), e seu filho esquerdo tem **FB = −1**.

#### Exemplo Antes e Depois da Rotação

Inserção de 30, 20, 10 (em ordem decrescente) desequilibra o nó 30:

```
ANTES (FB de C = -2):           DEPOIS (árvore equilibrada):

        C (30)                           B (20)
       /                                /      \
     B (20)           →             A (10)    C (30)
    /
  A (10)
```

**Passos executados:**
1. `B` (filho esquerdo de `C`) torna-se a nova raiz do subconjunto
2. `C` passa a ser o **filho direito** de `B`
3. A subárvore direita de `B` (se existir) passa a ser o **filho esquerdo** de `C`

---

### Rotação Simples à Esquerda

#### Objetivo

A rotação simples à esquerda tem como objetivo **reequilibrar** um nó cujo fator de balanceamento indica que a **subárvore direita está excessivamente alta**. É a operação simétrica (espelhada) da rotação à direita.

#### Situação em que é Utilizada

É aplicada no **caso RR (Right-Right):** o desequilíbrio ocorre porque foi inserido um elemento na subárvore direita do filho direito do nó desbalanceado.

Sinal de uso: o nó problemático possui **FB = +2**, e seu filho direito tem **FB = +1**.

#### Exemplo Antes e Depois da Rotação

Inserção de 10, 20, 30 (em ordem crescente) desequilibra o nó 10:

```
ANTES (FB de A = +2):           DEPOIS (árvore equilibrada):

  A (10)                                 B (20)
      \                                 /      \
      B (20)           →           A (10)    C (30)
          \
          C (30)
```

**Passos executados:**
1. `B` (filho direito de `A`) torna-se a nova raiz do subconjunto
2. `A` passa a ser o **filho esquerdo** de `B`
3. A subárvore esquerda de `B` (se existir) passa a ser o **filho direito** de `A`

---

### Rotação Dupla

Rotações duplas são necessárias quando o desequilíbrio ocorre em um **"cotovelo"** — ou seja, a inserção acontece no filho interno do nó desbalanceado. Uma única rotação não resolveria o problema; é preciso aplicar **duas rotações simples consecutivas**.

#### Esquerda-Direita (LR)

**Situação – caso LR:** inserção na subárvore **direita do filho esquerdo** do nó desbalanceado.  
O desequilíbrio forma um "cotovelo" que dobra para a esquerda e depois para a direita.

**Solução:**
1. Primeiro, aplicar **rotação simples à esquerda** no filho esquerdo
2. Depois, aplicar **rotação simples à direita** na raiz desbalanceada

```
ANTES:          PASSO 1 – rotação esquerda em A:    PASSO 2 – rotação direita em C:

    C                      C                                 B
   /                      /                                /   \
  A           →          B               →               A       C
   \                    /
    B                  A
```

Resultado: `B` torna-se raiz, `A` filho esquerdo, `C` filho direito — árvore equilibrada.

#### Direita-Esquerda (RL)

**Situação – caso RL:** inserção na subárvore **esquerda do filho direito** do nó desbalanceado.  
O desequilíbrio forma um "cotovelo" que dobra para a direita e depois para a esquerda.

**Solução:**
1. Primeiro, aplicar **rotação simples à direita** no filho direito
2. Depois, aplicar **rotação simples à esquerda** na raiz desbalanceada

```
ANTES:          PASSO 1 – rotação direita em C:     PASSO 2 – rotação esquerda em A:

  A                       A                                  B
   \                       \                                /   \
    C           →           B               →             A       C
   /                         \
  B                           C
```

Resultado: `B` torna-se raiz, `A` filho esquerdo, `C` filho direito — árvore equilibrada.

---

### Inversão (Espelhamento)

#### Conceito

A inversão (ou espelhamento) de uma árvore é uma operação que **troca recursivamente o filho esquerdo pelo filho direito** de cada nó da árvore, do nível mais interno até a raiz. O resultado é uma **imagem espelhada** da estrutura original: o que estava à esquerda vai para a direita e vice-versa, enquanto os valores de cada nó permanecem os mesmos.

#### Aplicação

- **Verificação de simetria:** determinar se duas árvores são imagens espelhadas entre si (problema clássico de entrevistas técnicas)
- **Algoritmos de comparação estrutural** em compiladores e ferramentas de análise de código
- **Visualização de árvores de decisão** em formas alternativas (ex: da direita para a esquerda)
- **Transformações de layouts** em interfaces gráficas baseadas em árvores (ex: espelhar um menu lateral)

#### Exemplo Antes e Depois da Operação

```
ANTES (original):                  DEPOIS (espelhada):

          4                                  4
         / \                                / \
        2   7          →                  7     2
       / \ / \                           / \   / \
      1  3 6  9                         9   6 3   1
```

**Algoritmo recursivo (pseudocódigo):**

```
função inverter(nó):
    se nó == null:
        retorne

    // Troca os filhos
    temp          ← nó.esquerda
    nó.esquerda   ← nó.direita
    nó.direita    ← temp

    // Aplica recursivamente nos dois lados
    inverter(nó.esquerda)
    inverter(nó.direita)
```

A operação visita cada nó exatamente uma vez, portanto sua complexidade é **O(n)**, onde *n* é o número de nós da árvore.

---

## Parte 3 – Aplicação Prática

### Sistema Gerenciador de Banco de Dados Relacional (SGBD)

**Estrutura escolhida: Árvore B+ (variação N-ária)**

#### Descrição do Sistema

Sistemas de banco de dados relacionais como **MySQL**, **PostgreSQL** e **Oracle** precisam localizar registros de forma extremamente rápida em tabelas que podem ter centenas de milhões de linhas. Os **índices** são a principal ferramenta para esse fim, e a estrutura de dados que implementa esses índices define diretamente o desempenho do banco.

#### Por que a Árvore B+ é a Mais Adequada?

**1. Acesso a disco otimizado**  
Bancos de dados armazenam dados em disco, e cada acesso ao disco é de 10.000 a 1.000.000 vezes mais lento que um acesso à RAM. A Árvore B+ agrupa muitas chaves em um único nó (do tamanho exato de um bloco de disco — normalmente 4 KB a 16 KB), minimizando drasticamente o número de leituras físicas necessárias por operação.

**2. Escalabilidade extrema com altura mínima**  
Uma B+ Tree com fator de ramificação de 1.000 e apenas **3 níveis** consegue indexar até **1 bilhão de registros** com no máximo 3 leituras de disco. Uma BST ou AVL equivalente exigiria 30 ou mais acessos para a mesma quantidade de dados.

**3. Varreduras de intervalo eficientes**  
Na B+ Tree, todos os dados reais ficam nas **folhas**, que são conectadas em uma **lista encadeada ordenada**. Isso torna consultas de intervalo (`WHERE salario BETWEEN 3000 AND 8000`) extremamente rápidas: basta localizar o ponto inicial e percorrer as folhas em sequência, sem precisar subir e descer na árvore.

**4. Balanceamento automático preservado**  
Inserções são gerenciadas com a operação de **split** (divisão de nó cheio), e remoções com **merge** (fusão de nós). Em ambos os casos, a árvore permanece perfeitamente balanceada e com altura garantidamente O(log N).

#### Comparação com as Outras Estruturas

| Estrutura       | Por que não é ideal para SGBD?                                                |
|-----------------|-------------------------------------------------------------------------------|
| BST             | Pode degenerar em lista encadeada; não é eficiente para acesso em bloco de disco |
| AVL             | Balanceamento rigoroso demais gera muitas rotações; nós pequenos aumentam acessos de I/O |
| Rubro-Negra     | Eficiente em memória RAM, mas não foi projetada para minimizar acessos em bloco de disco |
| **B+ Tree** ✅  | Projetada especificamente para armazenamento em blocos; minimiza I/O; suporta bilhões de registros |

#### Conclusão

Para um SGBD, a Árvore B+ é a estrutura mais adequada, razão pela qual é adotada universalmente pelos principais bancos de dados do mercado. A combinação de **baixo número de acessos ao disco**, **eficiência em consultas de intervalo** e **escalabilidade para bilhões de registros** torna essa estrutura insubstituível nesse contexto.

---

## Parte 4 – Comparação entre Estruturas

| Estrutura | Nº Máximo de Filhos | Balanceamento | Complexidade de Busca | Complexidade de Inserção | Vantagem Principal | Desvantagem Principal | Exemplo de Aplicação |
|-----------|:------------------:|:-------------:|:---------------------:|:------------------------:|-------------------|----------------------|---------------------|
| BST | 2 | Não possui balanceamento automático. Pode ficar desbalanceada conforme a ordem de inserção. | O(log n) no melhor caso e O(n) no pior caso | O(log n) no melhor caso e O(n) no pior caso | Estrutura simples de entender e implementar | Pode perder desempenho se ficar desbalanceada | Árvores de busca básicas |
| AVL | 2 | Sim. Mantém o fator de balanceamento com rotações | O(log n) | O(log n) | Busca previsível e eficiente | Inserção e remoção são mais custosas por causa das rotações | Índices e estruturas que exigem leitura frequente |
| Rubro-Negra | 2 | Sim. Usa coloração e rotações para manter o balanceamento aproximado | O(log n) | O(log n) | Balanceamento eficiente com menos rotações que a AVL | Implementação mais complexa | Bibliotecas e estruturas internas de sistemas |
| N-ária | N (vários filhos) | Pode ou não possuir mecanismos de balanceamento, dependendo da variação | O(log n) em estruturas balanceadas, mas depende da aplicação | O(log n) ou proporcional à estrutura da árvore | Representa melhor hierarquias com muitos filhos | Não é tão simples quanto a árvore binária em alguns contextos | Sistema de arquivos, menus e taxonomias |

### Análise da Tabela Comparativa

**BST (Árvore Binária de Busca)**  
A BST é a base conceitual de todas as outras estruturas desta tabela. Sua simplicidade é sua maior força: fácil de entender, implementar e depurar. No entanto, sem nenhum mecanismo de balanceamento, ela é vulnerável à **degeneração**: inserir dados em ordem crescente ou decrescente transforma a BST em uma lista encadeada, degradando todas as operações para O(n). Por isso, raramente é usada diretamente em sistemas de produção que exigem desempenho estável.

**AVL**  
A AVL resolve o problema de degeneração da BST com balanceamento rigoroso, garantindo que a diferença de altura entre subárvores esquerda e direita nunca ultrapasse 1. Isso assegura altura sempre O(log n) e torna a busca rápida e previsível. A contrapartida é o custo extra nas modificações: inserções e remoções podem exigir rotações em cascata pelo caminho da raiz até o nó alterado. É a escolha ideal para **sistemas com leitura intensiva**, como índices em memória de bancos de dados analíticos.

**Rubro-Negra**  
A Rubro-Negra aceita um balanceamento ligeiramente menos rigoroso que a AVL (o ramo mais longo pode ter até o dobro do mais curto), mas em compensação realiza **menos rotações por operação de modificação**. Isso a torna mais eficiente para **sistemas com inserções e remoções frequentes**. Não é à toa que é a estrutura escolhida para implementar mapas ordenados nas linguagens C++ (`std::map`) e Java (`TreeMap`), e também o escalonador de processos do kernel Linux.

**N-ária**  
A N-ária é a mais flexível e a mais poderosa para cenários com **muitos filhos por nó** ou **dados em disco**. A capacidade de ter N filhos reduz drasticamente a altura da árvore, tornando-a ideal para representar hierarquias naturais (como diretórios de sistema de arquivos) e para minimizar acessos de I/O em bancos de dados. As Árvores B e B+, variações N-árias amplamente conhecidas, são o núcleo dos sistemas de gerenciamento de banco de dados modernos. A desvantagem é a maior complexidade de implementação e o maior custo de memória por nó.
