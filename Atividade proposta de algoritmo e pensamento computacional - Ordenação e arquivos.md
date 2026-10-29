#include <stdio.h>
#include <stdlib.h>

// PROTÓTIPOS
void criarArquivoDados(const char *nomeArquivo);
int* lerArquivo(const char *nomeArquivo, int *n);
void salvarArquivo(const char *nomeArquivo, int *v, int n);

void bubbleSort(int v[], int n);
void insertionSort(int v[], int n);
void selectionSort(int v[], int n);
void mergeSort(int v[], int n);
void quickSort(int v[], int n);

// FUNÇÕES DE ARQUIVO
void criarArquivoDados(const char *nomeArquivo) {
    FILE *fp = fopen(nomeArquivo, "w");
    if (!fp) {
        printf("Erro ao criar arquivo!\n");
        return;
    }
    int n, val;
    printf("Quantos números deseja gravar? ");
    scanf("%d", &n);
    printf("Digite os %d números (separados por espaço ou enter):\n", n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &val);
        fprintf(fp, "%d\n", val);
    }
    fclose(fp);
    printf("Arquivo %s criado com %d números.\n", nomeArquivo, n);
}

int* lerArquivo(const char *nomeArquivo, int *n) {
    FILE *fp = fopen(nomeArquivo, "r");
    if (!fp) {
        printf("Erro ao abrir arquivo %s para leitura!\n", nomeArquivo);
        return NULL;
    }
    int count = 0, temp;
    while (fscanf(fp, "%d", &temp) == 1) count++;
    if (count == 0) {
        fclose(fp);
        printf("Arquivo vazio!\n");
        return NULL;
    }
    int *v = (int*) malloc(count * sizeof(int));
    if (!v) {
        fclose(fp);
        printf("Erro de alocação de memória!\n");
        return NULL;
    }
    rewind(fp);
    for (int i = 0; i < count; i++) {
        fscanf(fp, "%d", &v[i]);
    }
    fclose(fp);
    *n = count;
    return v;
}

void salvarArquivo(const char *nomeArquivo, int *v, int n) {
    FILE *fp = fopen(nomeArquivo, "w");
    if (!fp) {
        printf("Erro ao criar arquivo %s para escrita!\n", nomeArquivo);
        return;
    }
    for (int i = 0; i < n; i++) {
        fprintf(fp, "%d\n", v[i]);
    }
    fclose(fp);
    printf("Arquivo %s salvo com sucesso!\n", nomeArquivo);
}

// FUNÇÕES DE ORDENAÇÃO
void bubbleSort(int v[], int n) {
    int i, j, aux;
    printf("\n--- Bubble Sort ---\n");
    printf("Vetor inicial: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
    for (i = 0; i < n - 1; i++) {
        int trocou = 0;
        for (j = 0; j < n - i - 1; j++) {
            if (v[j] > v[j + 1]) {
                aux = v[j];
                v[j] = v[j + 1];
                v[j + 1] = aux;
                trocou = 1;
            }
        }
        printf("Passo %d: ", i+1);
        for (int k = 0; k < n; k++) printf("%d ", v[k]);
        printf("\n");
        if (!trocou) break;
    }
    printf("Vetor ordenado final: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
}

void insertionSort(int v[], int n) {
    int i, j, chave;
    printf("\n--- Insertion Sort ---\n");
    printf("Vetor inicial: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
    for (i = 1; i < n; i++) {
        chave = v[i];
        j = i - 1;
        while (j >= 0 && v[j] > chave) {
            v[j + 1] = v[j];
            j--;
        }
        v[j + 1] = chave;
        printf("Passo %d (inserindo %d): ", i, chave);
        for (int k = 0; k < n; k++) printf("%d ", v[k]);
        printf("\n");
    }
    printf("Vetor ordenado final: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
}

void selectionSort(int v[], int n) {
    int i, j, min, aux;
    printf("\n--- Selection Sort ---\n");
    printf("Vetor inicial: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
    for (i = 0; i < n - 1; i++) {
        min = i;
        for (j = i + 1; j < n; j++) {
            if (v[j] < v[min]) min = j;
        }
        if (min != i) {
            aux = v[i];
            v[i] = v[min];
            v[min] = aux;
        }
        printf("Passo %d (posição %d): ", i+1, i);
        for (int k = 0; k < n; k++) printf("%d ", v[k]);
        printf("\n");
    }
    printf("Vetor ordenado final: ");
    for (i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
}

/* --- Merge Sort --- */
void merge(int v[], int inicio, int meio, int fim) {
    int i = inicio, j = meio + 1, k = 0;
    int *aux = (int*) malloc((fim - inicio + 1) * sizeof(int));
    while (i <= meio && j <= fim) {
        if (v[i] <= v[j]) aux[k++] = v[i++];
        else aux[k++] = v[j++];
    }
    while (i <= meio) aux[k++] = v[i++];
    while (j <= fim) aux[k++] = v[j++];
    for (i = inicio, k = 0; i <= fim; i++, k++) v[i] = aux[k];
    free(aux);
    printf("Merge: [");
    for (i = inicio; i <= fim; i++) {
        printf("%d", v[i]);
        if (i < fim) printf(", ");
    }
    printf("] (índices %d a %d)\n", inicio, fim);
}

void mergeSortRec(int v[], int inicio, int fim) {
    if (inicio < fim) {
        int meio = (inicio + fim) / 2;
        mergeSortRec(v, inicio, meio);
        mergeSortRec(v, meio + 1, fim);
        merge(v, inicio, meio, fim);
    }
}

void mergeSort(int v[], int n) {
    printf("\n--- Merge Sort ---\n");
    printf("Vetor inicial: ");
    for (int i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
    mergeSortRec(v, 0, n - 1);
    printf("Vetor ordenado final: ");
    for (int i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
}

/* --- Quick Sort --- */
int partition(int v[], int inicio, int fim, int n) {
    int pivo = v[(inicio + fim) / 2];
    int i = inicio, j = fim;
    while (i <= j) {
        while (v[i] < pivo) i++;
        while (v[j] > pivo) j--;
        if (i <= j) {
            int aux = v[i];
            v[i] = v[j];
            v[j] = aux;
            i++;
            j--;
        }
    }
    printf("Partição (pivô %d): ", pivo);
    for (int k = 0; k < n; k++) printf("%d ", v[k]);
    printf("\n");
    return i;
}

void quickSortRec(int v[], int inicio, int fim, int n) {
    if (inicio < fim) {
        int pi = partition(v, inicio, fim, n);
        quickSortRec(v, inicio, pi - 1, n);
        quickSortRec(v, pi, fim, n);
    }
}

void quickSort(int v[], int n) {
    printf("\n--- Quick Sort ---\n");
    printf("Vetor inicial: ");
    for (int i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
    quickSortRec(v, 0, n - 1, n);
    printf("Vetor ordenado final: ");
    for (int i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");
}

// MAIN
int main() {
    const char *arquivoDados = "dados.txt";
    const char *arquivoOrdenado = "ordenado.txt";
    int opcao;
    int *v = NULL;
    int n = 0;

    do {
        printf("\nLABORATÓRIO DE ARQUIVOS E ORDENAÇÃO\n");
        printf("1 - Criar arquivo de dados (dados.txt)\n");
        printf("2 - Escolher algoritmo e ordenar (gera ordenado.txt)\n");
        printf("3 - Alterar um valor no arquivo ordenado e reordenar\n");
        printf("0 - Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                criarArquivoDados(arquivoDados);
                break;

            case 2: {
                v = lerArquivo(arquivoDados, &n);
                if (!v) break;
                int alg;
                printf("\nEscolha o algoritmo de ordenação:\n");
                printf("1 - Bubble Sort\n");
                printf("2 - Insertion Sort\n");
                printf("3 - Selection Sort\n");
                printf("4 - Merge Sort\n");
                printf("5 - Quick Sort\n");
                printf("Opção: ");
                scanf("%d", &alg);
                switch (alg) {
                    case 1: bubbleSort(v, n); break;
                    case 2: insertionSort(v, n); break;
                    case 3: selectionSort(v, n); break;
                    case 4: mergeSort(v, n); break;
                    case 5: quickSort(v, n); break;
                    default: printf("Opção inválida!\n"); free(v); continue;
                }
                salvarArquivo(arquivoOrdenado, v, n);
                free(v);
                v = NULL;
                break;
            }

            case 3: {
                v = lerArquivo(arquivoOrdenado, &n);
                if (!v) break;
                printf("\nValores atuais no arquivo ordenado:\n");
                for (int i = 0; i < n; i++) {
                    printf("%d: %d\n", i+1, v[i]);
                }
                int pos, novoValor;
                printf("Digite a posição (1 a %d) que deseja alterar: ", n);
                scanf("%d", &pos);
                if (pos < 1 || pos > n) {
                    printf("Posição inválida!\n");
                    free(v);
                    break;
                }
                printf("Digite o novo valor: ");
                scanf("%d", &novoValor);
                v[pos-1] = novoValor;

                int alg;
                printf("\nEscolha o algoritmo para reordenar:\n");
                printf("1 - Bubble Sort\n");
                printf("2 - Insertion Sort\n");
                printf("3 - Selection Sort\n");
                printf("4 - Merge Sort\n");
                printf("5 - Quick Sort\n");
                printf("Opção: ");
                scanf("%d", &alg);
                switch (alg) {
                    case 1: bubbleSort(v, n); break;
                    case 2: insertionSort(v, n); break;
                    case 3: selectionSort(v, n); break;
                    case 4: mergeSort(v, n); break;
                    case 5: quickSort(v, n); break;
                    default: printf("Opção inválida!\n"); free(v); continue;
                }
                salvarArquivo(arquivoOrdenado, v, n);
                free(v);
                v = NULL;
                break;
            }

            case 0:
                printf("Saindo...\n");
                break;

            default:
                printf("Opção inválida!\n");
        }
    } while (opcao != 0);

    return 0;
}