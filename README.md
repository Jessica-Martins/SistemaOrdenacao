# SistemaOrdenacao
Sistema de Ordenação de vetor em C++
#include <stdio.h>
#include <stdlib.h>
#include <locale.h> /* aceitação de caracteres especiais*/
#include <sys/time.h> /*mostra o tempo de execução*/
#define MAX 1000 /* definir tamanho do vetor*/

void aleatorio (int *vetor, size_t n){ /*criação do vetor */
	
	if (n > 1) {
		size_t i;
		
		for (i = 0; i < MAX; i++){
			
			size_t j = i + rand() / (RAND_MAX / (n-i) + 1);
			int k = vetor [j];
			vetor [j] = vetor [i];
			vetor [i] = k;
		}
	}
}

void ord_Bolha (int vetor [], int n){ /* método de ordenação bubblesort*/
	int i, j, aux;
	
	for (i = 0; i < n; i++){
		
		for (j = 0; j < n-1; j++){
			
			if (vetor [j] > vetor [j+1]){
				aux = vetor [j];
				vetor [j] = vetor [j+1];
				vetor [j+1] = aux;
			}
		}
	}
}

void ord_Insercao (int vetor [], int n ){ /* método de ordenação insertionsort*/
	int i, j, aux;
	
	for (i = 1; i < n; i++){
		aux = vetor[i];
		
		for (j = i-1; (j >= 0) && (aux < vetor [j]); j--){
			vetor [j+1] = vetor [j];
		}
		
		vetor [j+1] = aux;
	}
}

void ord_Selecao (int vetor[], int n){ /* método de ordenação selectionsort*/
	int i, j, menor, transicao;
	
	for (i = 0; i < n-1; i++){
		menor = i;
		
		for (j = i+1; j < n; j++){
			if (vetor [j] < vetor [menor]){
				menor = j;
			}
		}
		
		if ( i != menor) {
			transicao = vetor [i];
			vetor [i] = vetor [menor];
			vetor [menor] = transicao;
		}
	}
}
void constroi_Heap (int *vetor, int ponto_Inicial, int ponto_Final); /* construção da função protótipo do método de ordenação heapsort*/

void ord_Heap (int vetor [], int n){ /* método de ordenação heapsort*/
	int i, aux;
	
	for (i = (n-1)/2; i >= 0; i--)	{
		constroi_Heap (vetor, i, n-1);
	}
	
	for (i = n -1; i >= 1; i--){
		aux = vetor[0];
		vetor[0] = vetor [i];
		vetor [i] = aux;
		constroi_Heap (vetor, 0, i-1);
	}
}

void constroi_Heap (int *vetor, int ponto_Inicial, int ponto_Final){ /* Função do método de ordenação heapsort*/
	int aux = vetor [ponto_Inicial];
	int j = ponto_Inicial * 2+1;
	
	while (j <= ponto_Final){
		if (j < ponto_Final){
			if (vetor [j] < vetor [j+1] ){
				j = j+1;
			}
		}
		if ( aux < vetor [j]) {
			vetor [ponto_Inicial] = vetor [j];
			ponto_Inicial = j;
			j = 2 * ponto_Inicial+1;
		}
		else {
			j = ponto_Final+1;
		}
	}
	vetor [ponto_Inicial] = aux;
}


void constroi_Intercalacao (int vetor[], int posicao_Inicial, int posicao_Meio, int posicao_Final, int aux[]); /* construção da função protótipo do método de ordenação mergesort*/

void ord_Intercalacao (int vetor[], int posicao_Inicial, int posicao_Final, int aux []){ /* Função do método de ordenação mergesort*/
	int posicao_Meio = (posicao_Final + posicao_Inicial)/2;
	
	if (posicao_Inicial < posicao_Final){
		ord_Intercalacao (vetor, posicao_Inicial, posicao_Meio, aux);
		ord_Intercalacao (vetor, posicao_Meio + 1, posicao_Final, aux);
		constroi_Intercalacao (vetor, posicao_Inicial, posicao_Meio, posicao_Final, aux);
	}
}

void constroi_Intercalacao (int vetor[], int posicao_Inicial, int posicao_Meio, int posicao_Final, int aux[]){
	int i = posicao_Inicial, j = posicao_Meio +1, k = 0;
	
	while (i <= posicao_Meio && j <= posicao_Final){
		if (vetor [i] <= vetor [j]){
			aux [k++] = vetor [i++];
		}
		
		else {
			aux [k++] = vetor [j++];
		}
	}
	while (i <= posicao_Meio)
	aux [k++] = vetor [i++];
	
	while (j <= posicao_Final)
	aux [k++] = vetor [j++];
	for (i = posicao_Inicial, k=0; i<= posicao_Final; i++, k++)
		vetor [i] = aux [k]; /*o vetor auxiliar foi copiado para "vetor"*/
}

int det_Pivo (int i, int j); /* construção da função protótipo para determinar o valor pivô do vetor, no método de ordenação quicksort*/

void troca (int *x, int *y); /* construção da função protótipo para trova de valores do vetor, no método de ordenação quicksort*/
void ord_Quick (int vetor [], int primeiro, int ultimo){ /* Função do método de ordenação quicksort*/
	int i, j, k, l;
	
	if(primeiro < ultimo){
		k = det_Pivo (primeiro, ultimo);
		troca (&vetor [primeiro], &vetor[k]);
		l = vetor [primeiro];
		i = primeiro+1;
		j = ultimo;
		
		while (i <= j) {
			while ((i <= ultimo) && (vetor [i] <= l))
				i++;
			while ((j >= primeiro) && (vetor [j] > l))
				j--;
				
				if ( i < j)
				troca (&vetor [i], & vetor[j]);
		}
		
		troca (&vetor [primeiro], & vetor[j]);
		ord_Quick (vetor, primeiro, j-1);
		ord_Quick (vetor, j+1, ultimo);
	}
}

int det_Pivo (int i, int j){
	return ((i + j) / 2);
}

void troca (int *x, int *y){
	int tp;
    tp = *x;
    *x = *y;
    *y = tp;
}


int main () {
	
	setlocale (LC_ALL, "Portuguese");
	
	struct timeval tp_fim, tp_inicio;
	int vetor[MAX],i, aux [MAX];
	int op;
	double tempo_Bolha = 0, tempo_Insercao = 0, tempo_Selecao = 0, tempo_Heap = 0, tempo_Intercalacao = 0, tempo_Quick = 0;

        printf ("\n\n\t***** SISTEMA DE ORDENAÇÃO *****    \n\n\n");
        
        do{
        	printf ("\n\n \t\t ESCOLHA A OPÇÃO \n\n\n");
        	
            printf ("\t1 - Gerar números aleatórios \n");
            printf ("\t2 - Ordenação em Bolha (Bubblesort) \n");
            printf ("\t3 - Ordenação em Insercao (Insertion) \n");
            printf ("\t4 - Ordenação em Seleção (Selectionsort) \n");
            printf ("\t5 - Ordenação em Heapsort \n");
            printf ("\t6 - Ordenação em Intercalação (Mergesort) \n");
            printf ("\t7 - Ordenação em Quicksort \n");
            printf ("\t8 - Tempo de todas ordenações \n");
            printf ("\t9 - Sair \n\n");
            
            scanf ("%d", &op);
            
            	switch (op){
            		
            		case 1:
            			
            			srand (time (NULL));
            			
						for (i=0; i < MAX; i++ ){
            				vetor[i] = i;
						}
						
						aleatorio (vetor, MAX);
						
						for (i=0; i < MAX; i++){
							printf ("%d \t", vetor [i]);
						}
						
						break;
						
					case 2: 
						
						gettimeofday (&tp_inicio, NULL);
						ord_Bolha (vetor, MAX);
						gettimeofday (&tp_fim, NULL);
						tempo_Bolha = (double)(tp_fim.tv_usec - tp_inicio.tv_usec)/1000000 + (double) (tp_fim.tv_sec - tp_inicio.tv_sec);
						
						for (i=0; i< MAX; i++){
							printf ("%d \t", vetor [i]);
						}
							
						printf ("\n\n %f milésimos de segundo para ordenar %d números, com o método de ordenação bolha (bubblesort) ", tempo_Bolha, MAX);
							
						break;
							
					case 3:
							
						gettimeofday (&tp_inicio, NULL);
						ord_Insercao (vetor, MAX);
						gettimeofday (&tp_fim, NULL);
						tempo_Insercao = (double) (tp_fim.tv_usec - tp_inicio.tv_usec)/(1000000) + (double)(tp_fim.tv_sec - tp_inicio.tv_sec);
							
						for (i=0; i< MAX; i++){
							printf ("%d \t", vetor[i]);
						}
						printf ("\n\n %f milésimos de segundo de %d números com método de ordenação inserção (Insertionsort)", tempo_Insercao, MAX);
								
						break;
								
					case 4:
						
						gettimeofday (&tp_inicio, NULL);
						ord_Selecao (vetor, MAX);
						gettimeofday (&tp_fim, NULL);
						tempo_Selecao = (double) (tp_fim.tv_usec - tp_inicio.tv_usec)/1000000 + (double)(tp_fim.tv_sec - tp_inicio.tv_sec);
						
						for (i = 0; i < MAX; i++){
							printf ("%d \t ", vetor[i]);
						}
						
						printf ("\n\n %f milésimos de segundo de %d números com método de seleção (selectionsort)", tempo_Selecao, MAX);
						break;
									
					case 5:
			
						gettimeofday (&tp_inicio, NULL);
						ord_Heap (vetor, MAX);
						gettimeofday (&tp_fim, NULL);
						tempo_Heap = (double)(tp_fim.tv_usec - tp_inicio.tv_usec)/1000000+ (double) (tp_fim.tv_sec - tp_inicio.tv_sec);
								
						for (i=0; i<MAX; i++){
							printf ("%d \t", vetor [i]);
						}
						
						printf ("\n\n %f milésimos de segundo de %d números com o método heapsort\n\n", tempo_Heap, MAX);
									
						break;
									
					case 6:
						
						gettimeofday (&tp_inicio, NULL);
						ord_Intercalacao (vetor, 0, MAX, aux);
						gettimeofday (&tp_fim, NULL);
						tempo_Intercalacao = (double)(tp_fim.tv_usec - tp_inicio.tv_usec)/1000000 + (double)(tp_fim.tv_sec-tp_inicio.tv_sec);
						
						for (i=0; i < MAX; i++){
							printf ("%d \t", vetor [i]);
						}
						printf ("\n\n %f milésimos de segundo de %d números com o método de intercalacao (Merge sort) \n\n", tempo_Intercalacao, MAX);
						
						break;
						
					case 7:
						
						gettimeofday (&tp_inicio, NULL);
						ord_Quick (vetor, 0, MAX - 1);
						gettimeofday (&tp_fim, NULL);
						tempo_Quick = (double) (tp_fim.tv_usec - tp_inicio.tv_usec)/1000000 + (double) (tp_fim.tv_sec - tp_inicio.tv_sec);
						
						for (i = 0; i < MAX; i++){
							printf ("%d \t", vetor [i]);
							
						}
						
						printf ("\n\n %f milésimos de segundo de %d números com o método de ordenação quick sort \n\n", tempo_Quick, MAX);
						
						break;
					
					case 8:
						
						system ("cls");
						printf ("Tempo do método de ordenação em bolha (Bubblesort): %f  milésimos de segundo\n\n", tempo_Bolha);
                        printf ("Tempo do método de ordenação em inserção (Insertionsort): %f  milésimos de segundo\n\n", tempo_Insercao);
                        printf ("Tempo do método de ordenação em seleção (Selectionsort): %f  milésimos de segundo\n\n", tempo_Selecao);
                        printf ("Tempo do método de ordenação em Heapsort: %f  milésimos de segundo\n\n", tempo_Heap);
                        printf ("Tempo do método de ordenação em Intercalação (Mergesort): %f  milésimos de segundo\n\n", tempo_Intercalacao);
                        printf ("Tempo do método de ordenação em Quicksort: %f  milésimos de segundo\n\n", tempo_Quick);
                        
                        break;
                        
                    case 9:
                    	
                  	printf ("\n\n\t PROGRAMA FINALIZADO!\n");
                  	exit(1);
						
                    default:
					printf("\n\n Opção Inválida! \n\n");
				}
									
		} while (op != 9);
}
