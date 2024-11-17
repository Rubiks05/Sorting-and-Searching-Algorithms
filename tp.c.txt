#include <stdio.h>
#include <stdlib.h>

long long count_comparacoes;
long long count_trocas;
int vector_das_comparacoes[1000];

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA A FUNÇÃO DE SORTEIO QUE SERÁ USADA NOS ALGORITMOS ABAIXO.
long aleat (long min, long max) {

	return min + rand() % (max - min + 1);

}

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA IMPRIMIR UMA PARTE DO VETOR.
// imprimindo os 100 primeiros elementos.
void imprime_parte(int vector[]) {

	for (int i=0 ; i<100 ; i++) printf("%d ", vector[i]);
	printf("\n");
}


/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA SE FAZER A FUNÇÃO "TROCA" QUE SERÁ USADA NOS ALGORITMOS ABAIXO.
void troca(int *xp, int *yp) {

  	int temp = *xp;
  	*xp = *yp;
  	*yp = temp;
}

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA SE FAZER O ALGORITMO DE SELECTION SORT.
int selection_sort(int vetor[], int n) {
	
  	int i,j,min_indx;

 	for (i = 0 ; i < (n-1) ; i++) {

 		min_indx = i;
 		for (j = i+1 ; j < n ; j++) {
			count_comparacoes++; 
 			if (vetor[j] < vetor[min_indx]) min_indx = j;
 		}
	
 		troca(&vetor[min_indx], &vetor[i]);
		count_trocas++;
 	}
	return count_comparacoes;
}

/*------------------------------------------------------------------------------*/
//PARTE SELECIONADA PARA SE CRIAR UM VETOR.
//Copia feita para se fazer a pesquisa sequencial.
void cria_vector(int vector[], int copia_vector[]) {
	
	for (int i=0 ; i<1024 ; i++) {
		vector[i] = aleat(0,2048);
		copia_vector[i] = vector[i];
  	}
	// printf("Vetor Criado, para exibir uma parte dele, digite 2.\n");
}

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA SE FAZER O ALGORITMO DE QUICK SORT.
int mediano(int a, int b, int c) {
	if ((a > b && a < c) || (a < b && a > c)) return a;
	else if ((b > a && b < c) || (b < a && b > c)) return b;
	else return c;
}


int particao(int vector[], int low, int high, int escolha_do_pivo) {

	int pivo, pivo_index = high;

	//Forma 1 de se escolher o pivô.
	if (escolha_do_pivo == 1) pivo = vector[high];
	// Forma 2 de se escolher o pivô.
	else if (escolha_do_pivo == 2) {
		int meio = low + (high - low) / 2;
		pivo = mediano(vector[low], vector[meio], vector[high]);

		// Encontrar o índice do pivô e movê-lo para a posição final.
                if (pivo == vector[low]) pivo_index = low;
                else if (pivo == vector[meio]) pivo_index = meio;
                // Se o pivô já for o último, pivo_index = high.

                // Mover o pivô para a posição final.
                troca(&vector[pivo_index], &vector[high]);
	}

	int i = low - 1;

	for (int j = low; j < high; j++) {
		count_comparacoes++;
		if (vector[j] <= pivo) {
			i++;
			troca(&vector[i], &vector[j]);
			count_trocas++;
		}
	}

	troca(&vector[i + 1], &vector[high]);
	count_trocas++;
         
	// retorna o indice do vetor.
	return i + 1;
}

void quick_sort(int vector[], int low, int high, int escolha_do_pivo) {
	
	if (low < high) {
		int pi = particao(vector, low, high, escolha_do_pivo);

		quick_sort(vector, low, pi - 1, escolha_do_pivo);
		quick_sort(vector, pi + 1, high, escolha_do_pivo);
	}
}

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA SE FAZER O ALGORITMO DE SHELL SORT.
void shell_sort(int vector[], int n, int escolha_espacamento) {

	int gap;

	if (escolha_espacamento == 1) {
		for (gap = n / 2; gap > 0; gap /= 2) {
			for (int i = gap; i < n; i++) {
				int temp = vector[i];
				int j;
				for (j = i; j >= gap; j -= gap) {
					count_comparacoes++;
					if (vector[j - gap] <= temp) break;
						vector[j] = vector[j - gap];
						count_trocas++;
					}
				vector[j] = temp;
				if (j != i) count_trocas++;
			}
		}
	} 
	else if (escolha_espacamento == 2) {
		gap = 1;
		while (gap < n / 3) gap = 3 * gap + 1;
		while (gap > 0) {
			for (int i = gap; i < n; i++) {
				int temp = vector[i];
				int j;
				for (j = i; j >= gap; j -= gap) {
					count_comparacoes++;
					if (vector[j - gap] <= temp) break;
					vector[j] = vector[j - gap];
					count_trocas++;
				}
				vector[j] = temp;
				if (j != i) count_trocas++;
			}
			gap /= 3;
		}
	}
}

/*------------------------------------------------------------------------------*/
// PARTE SELECIONADA PARA SE FAZER A PESQUISA SEQUENCIAL.
int pesquisa_sequencial(int copia_vector[], int escolha_pesquisa, int tamanho, int entrada) {
	
	if (escolha_pesquisa == 1 || escolha_pesquisa == 2) {
		for (int i=0 ; i<tamanho ; i++) {
			if (copia_vector[i] == entrada) return i;
			count_comparacoes++;
		}
		// printf("Elemento %d não encontrado\n", entrada);
		return -1;
	}

	else {
		printf("Entrada não compreendida. Caso deseja fazer a pesquisa sequencial, aperte 6 novamente!\n");
		return -1;
	}
}
/*------------------------------------------------------------------------------*/
//PARTE SELECIONADA PARA SE FAZER A PESQUISA BINÁRIA.
int pesquisa_binaria(int vector[], int escolha_pesquisa, int tamanho, int entrada) {
	int inicio = 0;
	int fim = tamanho-1;

	while (inicio <= fim) {
		int meio = inicio + (fim-inicio) / 2;
		count_comparacoes++;

		if (vector[meio] == entrada) return meio;

		if (vector[meio] < entrada) inicio = meio+1;

		else fim = meio-1;
	}
	return -1;
}
/*------------------------------------------------------------------------------*/
//PARTE SELECIONADA PARA SE FAZER AS BUSCAS E ORDENAÇÕES 1000 VEZES DE TODOS.
int raiz_quadrada(int numero) {
//Ajuda no cálculo do desvio padrão.
	
	if (numero <= 0) return 0;
	int x = numero;
	int y = (x + 1) / 2;
	while (y < x) {
		x = y;
		y = (x + numero / x) / 2;
	}
	return x;
}

int media(int vector_das_comparacoes[]) {
	
	long long soma = 0;
	for (int i=0 ; i<1000 ; i++) soma += vector_das_comparacoes[i];

	long long media_k = soma / 1000;

	return media_k;

}
int desvio_padrao(int vector_das_comparacoes[]) {
	
	long long media_s = media(vector_das_comparacoes);
	
	long long soma_para_variancia = 0;
	for (int i=0 ; i<1000 ; i++) {
		soma_para_variancia += (vector_das_comparacoes[i] - media_s) * (vector_das_comparacoes[i] - media_s);
	}
	long long variancia = soma_para_variancia / 1000;

	return raiz_quadrada(variancia);
}

void mil_selection_sort() {
	
	int vector_s[1024], copia_vector_s[1024];
	count_comparacoes = 0;
	for (int i=0 ; i<1000 ; i++) {
		cria_vector(vector_s, copia_vector_s);
		vector_das_comparacoes[i] = selection_sort(vector_s, 1024);
		count_comparacoes = 0;
	}
	
	long long media_selectionsort = media(vector_das_comparacoes);
	long long desvio_padrao_selectionsort = desvio_padrao(vector_das_comparacoes);

	printf("Média do Selection Sort: %lld\n", media_selectionsort);
	printf("Desvio padrão do Selection Sort: %lld\n", desvio_padrao_selectionsort);
}

void mil_quick_sort_ultimoelemento() {

	int vector_q[1024], copia_vector_q[1024];
	count_comparacoes = 0;
	for (int i=0 ; i<1000 ; i++) {
		cria_vector(vector_q, copia_vector_q);
		quick_sort(vector_q, 0, 1023, 1);
		vector_das_comparacoes[i] = count_comparacoes;
		count_comparacoes = 0;
	}

	long long media_quicksort = media(vector_das_comparacoes);
	long long desvio_padrao_quicksort = desvio_padrao(vector_das_comparacoes);

	printf("Média do Quick Sort último elemento: %lld\n", media_quicksort);
	printf("Desvio padrao do Quick Sort último elemento: %lld\n", desvio_padrao_quicksort);
}

void mil_quick_sort_mediano() {

        int vector_q[1024], copia_vector_q[1024];
        count_comparacoes = 0;
        for (int i=0 ; i<1000 ; i++) {
                cria_vector(vector_q, copia_vector_q);
                quick_sort(vector_q, 0, 1023, 2);
                vector_das_comparacoes[i] = count_comparacoes;
                count_comparacoes = 0;
        }

        long long media_quicksort = media(vector_das_comparacoes);
        long long desvio_padrao_quicksort = desvio_padrao(vector_das_comparacoes);

        printf("Média do Quick Sort mediano: %lld\n", media_quicksort);
        printf("Desvio padrao do Quick Sort mediano: %lld\n", desvio_padrao_quicksort);
}

void mil_shell_sort_padrao() {

        int vector_ss[1024], copia_vector_ss[1024];
        count_comparacoes = 0;
        for (int i=0 ; i<1000 ; i++) {
                cria_vector(vector_ss, copia_vector_ss);
                shell_sort(vector_ss, 1024, 2);
                vector_das_comparacoes[i] = count_comparacoes;
                count_comparacoes = 0;
        }

        long long media_shellsort = media(vector_das_comparacoes);
        long long desvio_padrao_shellsort = desvio_padrao(vector_das_comparacoes);

        printf("Média do Shell Sort por espaçamento padrão: %lld\n", media_shellsort);
        printf("Desvio padrao do Shell Sort por espaçamento padrão: %lld\n", desvio_padrao_shellsort);
}

void mil_shell_sort_knuth() {

	int vector_ss[1024], copia_vector_ss[1024];
	count_comparacoes = 0;
	for (int i=0 ; i<1000 ; i++) {
		cria_vector(vector_ss, copia_vector_ss);
		shell_sort(vector_ss, 1024, 2);
		vector_das_comparacoes[i] = count_comparacoes;
		count_comparacoes = 0;
	}
	
	long long media_shellsort = media(vector_das_comparacoes);
	long long desvio_padrao_shellsort = desvio_padrao(vector_das_comparacoes);

	printf("Média do Shell Sort por espaçamento de Knuth: %lld\n", media_shellsort);
	printf("Desvio padrao do Shell Sort por espaçamento de Knuth: %lld\n", desvio_padrao_shellsort);
}

void mil_pesquisa_sequencial() {

	int vector_ps[1024], copia_vector_ps[1024];
	count_comparacoes = 0;
	int entrada_s;
	for (int i=0 ; i<1000 ; i++) {
		cria_vector(vector_ps, copia_vector_ps);
		entrada_s = aleat(0,2048);
		pesquisa_sequencial(copia_vector_ps, 2, 1024, entrada_s);
		vector_das_comparacoes[i] = count_comparacoes;
		count_comparacoes = 0;
	}

	long long media_pesquisasequencial = media(vector_das_comparacoes);
	long long desvio_padrao_pesquisasequencial = desvio_padrao(vector_das_comparacoes);

	printf("Média da Pesquisa Sequencial: %lld\n", media_pesquisasequencial);
	printf("Desvio padrão da Pesquisa Sequencial: %lld\n", desvio_padrao_pesquisasequencial);
}

void mil_pesquisa_binaria() {

	int vector_pb[1024], copia_vector_pb[1024];
	count_comparacoes = 0;
	int entrada_b;
	for (int i=0 ; i<1000 ; i++) {
		cria_vector(vector_pb, copia_vector_pb);
		entrada_b = aleat(0,2048);
		//Ordenando o vetor para não dar ruim(pelo melhor método de ordenação).
		quick_sort(vector_pb, 0,1023,2);
		//Zerando count_comparacoes que foi modificado na função do Quick Sort.
		count_comparacoes = 0;
		pesquisa_binaria(vector_pb,2,1024,entrada_b);
		vector_das_comparacoes[i] = count_comparacoes;
		count_comparacoes = 0;
	}
	
	long long media_pesquisabinaria = media(vector_das_comparacoes);
	long long desvio_padrao_pesquisabinaria = desvio_padrao(vector_das_comparacoes);

	printf("Média da Pesquisa Binária: %lld\n", media_pesquisabinaria);
	printf("Desvio Padrão da Pesquisa Binária: %lld\n", desvio_padrao_pesquisabinaria);

}

/*------------------------------------------------------------------------------*/

int main() {
	
	printf("-------------------------------------------------------------\n");
	printf("# MENU DO MEU ALGORITMO: \n");
	printf("\n");
	printf("ATENÇÃO: NÃO APERTE NENHUM OUTRO BOTÃO NÃO PEDIDO AO LONGO DO PROGRAMA.\n");
	printf("Se você apertar, REINICIE O PROGRAMA.\n");
	printf("\n");
	printf("1 - Cria um novo vetor.\n");
	printf("2 - Imprime 100 primeiros elementos.\n");
	printf("3 - Ordena vetor Selection Sort.\n");
	printf("4 - Ordena vetor Quick Sort.\n");
	printf("        1 - Último elemento.\n");
	printf("        2 - Elemento mediano entre o primeiro, o meio e o último elemento do vetor.\n");
	printf("5 - Ordena vetor Shell Sort.\n");
	printf("        1. Espaçamento Padrão divindo por 2.\n");
	printf("        2. Espaçamento de Knuth (3×gap+1 até ser menor que n/3).\n");
	printf("6 - Pesquisa Sequencial:\n");
	printf("        1 - Usuário Escolhe.\n");
	printf("        2 - Gerado Aleatóriamente.\n");
	printf("7 - Pesquisa Binária:\n");
	printf("        1 - Usuário Escolhe.\n");
	printf("        2 - Gerado Aleatóriamente.\n");
	printf("8 - Ordenações e Buscas de TODOS 1000 vezes.\n");
	printf("9 - Caso deseje encerrar o programa.\n");
	printf("-------------------------------------------------------------\n");
	printf("\n");



	int vector[1023];

	//A fim da pesquisa sequencial ser feita no vetor não ordenado, cria-se:
	int copia_vector[1023];

	printf("Digite a Entrada: ");
	int entrada;

	
	for (int i=0 ; i<1000 ; i++) {

		scanf("%d", &entrada);

		if (entrada == 1) {
			cria_vector(vector, copia_vector);
			printf("Vetor Criado, para exibir uma parte dele, digite 2.\n");
		}
		else if (entrada == 2) imprime_parte(vector);
		else if (entrada == 3) {
			selection_sort(vector, 1024);
			printf("Número de Comparações: %lld\n", count_comparacoes);
			printf("Número de Trocas: %lld\n", count_trocas);
			imprime_parte(vector);
			//inicializando as variáveis de comparações e trocas novamente para 0:
			count_comparacoes = 0;
			count_trocas = 0;
		}
		else if (entrada == 4) {
			int escolha_do_pivo;
			printf("Escolha a forma de escolher o pivô: \n");
			printf("1. Escolhendo o último elemento.\n");
			printf("2. Pegando 3 elementos e escolhendo o do meio.\n");
			scanf("%d", &escolha_do_pivo);

			quick_sort(vector, 0, 1023, escolha_do_pivo);
			printf("Número de Comparações: %lld\n", count_comparacoes);
			printf("Número de Trocas: %lld\n", count_trocas);
			imprime_parte(vector);
			
			//inicializando as variáveis de comparações e trocas novamente para 0:
			count_comparacoes = 0;
			count_trocas = 0;
		}
		else if (entrada == 5) {
			int escolha_espacamento;
			printf("Escolha a forma de escolher o espaçamento: \n");
			printf("1. Espaçamento Padrão divindo por 2.\n");
			printf("2. Espaçamento de Knuth (3×gap+1 até ser menor que n/3).\n");
			scanf("%d", &escolha_espacamento);

			shell_sort(vector, 1024, escolha_espacamento);

			printf("Número de Comparações: %lld\n", count_comparacoes);
			printf("Número de Trocas: %lld\n", count_trocas);
			imprime_parte(vector);

			//inicializando as variáveis de comparações e trocas novamente para 0:
			count_comparacoes = 0;
			count_trocas = 0;
		}
		else if (entrada == 6) {
			int escolha_pesquisa;
			printf("Escolha a forma de se fazer a pesquisa: \n");
			printf("1. Elemento que usuário escolhe.\n");
			printf("2. Elemento gerado aleatóriamente\n");
			scanf("%d", &escolha_pesquisa);
			
			int entrada_s = 0;
			if (escolha_pesquisa == 1) {
				printf("Escolha um elemento para ser pesquisado: ");
				scanf("%d", &entrada_s);
			}
			if (escolha_pesquisa == 2) {
				entrada_s = aleat(0, 2048);
			}
			
			int pesquisa_s = pesquisa_sequencial(copia_vector, escolha_pesquisa, 1024, entrada_s);
			printf("Número de Comparações feitas: %lld\n", count_comparacoes);

			//inicializando a variável de comparação novamente para 0.
			count_comparacoes = 0;

			if (pesquisa_s != -1) printf("Elemento %d encontrado no índice %d do vetor.\n", entrada_s, pesquisa_s);
			else printf("Elemento %d não encontrado no vetor.\n", entrada_s);

		}
		else if (entrada == 7) {
			printf("ATENÇÃO, SE VOCÊ NÃO ORDENOU O VETOR ANTERIORMENTE NÃO DARÁ CERTO A PESQUISA BINÁRIA.\n");
			printf("Se você ordenou e deseja seguir, aperte 1.\nCaso não ordenou, aperte qualquer tecla e escolha uma opção para ordenar.\n");
			int sera_que_ordenou;
			scanf("%d", &sera_que_ordenou);
			if (sera_que_ordenou == 1) {
				int escolha_pesquisa;
				printf("Escolha a forma de se fazer a pesquisa: \n");
				printf("1. Elemento que usuário escolhe.\n");
				printf("2. Elemento gerado aleatóriamente\n");
				scanf("%d", &escolha_pesquisa);

				int entrada_b = 0;
				if (escolha_pesquisa == 1) {
					printf("Escolha um elemento para ser pesquisado: ");
					scanf("%d", &entrada_b);
				}
				if (escolha_pesquisa == 2) {
					entrada_b = aleat(0,2048);
				}

				int pesquisa_b = pesquisa_binaria(vector, escolha_pesquisa, 1024, entrada_b);
				printf("Número de Comparações feitas: %lld\n", count_comparacoes);

				//inicializando a variável de comparação novamente para 0.
				count_comparacoes = 0;

				if (pesquisa_b != -1) printf("Elemento %d encontrado no índice %d do vetor.\n", entrada_b, pesquisa_b);
				else printf("Elemento %d não encontrado no vetor.\n", entrada_b);
			}
		}
		else if (entrada == 8) {
			printf("--------------------------------------------------\n");
			printf("\n");
			printf("# MÉDIAS E DESVIOS PADRÕES DAS COMPARAÇÕES DOS ALGORITMOS: \n");
			printf("\n");
			printf("--------------------------------------------------\n");
			mil_selection_sort();
			printf("--------------------------------------------------\n");
			mil_quick_sort_ultimoelemento();
			printf("--------------------------------------------------\n");
			mil_quick_sort_mediano();
			printf("--------------------------------------------------\n");
			mil_shell_sort_padrao();
			printf("--------------------------------------------------\n");
			mil_shell_sort_knuth();
			printf("--------------------------------------------------\n");
			mil_pesquisa_sequencial();
			printf("--------------------------------------------------\n");
			mil_pesquisa_binaria();
			printf("--------------------------------------------------\n");
		}
		else if (entrada == 9) break;
	}
}
