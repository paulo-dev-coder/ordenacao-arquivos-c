# Laboratório de ordenação e arquivos em C

Este repositório contém um projeto prático e didático desenvolvido em **linguagem C** que unifica conceitos fundamentais de programação. O programa consiste em um menu interativo que gerencia dados numéricos em arquivos de texto, aplica algoritmos de ordenação e exibe o passo a passo (log) de cada iteração.

O projeto foi baseado na integração dos seguintes tópicos de estudo:
*   **Manipulação de Arquivos** (`fopen`, `fprintf`, `fscanf`, `fclose`);
*   **Ponteiros e Alocação Dinâmica** (`malloc`, `free`, aritmética de ponteiros);
*   **Algoritmos de Ordenação** (Bubble, Insertion, Selection, Merge e Quick Sort);
*   **Estruturas de Controle** (Menu interativo com `switch/case` e `do-while`).

---

## Funcionalidades

O programa apresenta um menu principal com as seguintes opções:

1.  **Criar arquivo de dados (`dados.txt`)**: Solicita ao usuário a quantidade e os valores numéricos, gravando-os no arquivo de entrada.
2.  **Ordenar dados**: Lê o arquivo `dados.txt`, permite escolher um dos 5 algoritmos de ordenação, **exibe no console o vetor passo a passo** (mostrando como o algoritmo está trabalhando) e salva o resultado final no arquivo `ordenado.txt`.
3.  **Alterar e Reordenar**: Lê o arquivo `ordenado.txt`, permite que o usuário altere um valor em uma posição específica, escolha um algoritmo de ordenação, visualize o novo passo a passo e salve o resultado atualizado.
4.  **Sair**: Encerra a execução do programa.

---

## Como compilar e executar

Para compilar e rodar o projeto, você precisará de um compilador C (como o `gcc`) instalado em sua máquina.

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/seu-usuario/lab-ordenacao-arquivos-c.git
   cd lab-ordenacao-arquivos-c
