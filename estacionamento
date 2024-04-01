#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_VAGAS 3
#define ISENTO 5
#define ATE_10_MIN 10
#define ATE_30_MIN 30
#define ATE_1_HORA 60
#define ATE_2_HORAS 120
#define APOS_3_HORAS 180
#define VALOR_ATE_10_MIN 7.5
#define VALOR_ATE_30_MIN 15.0
#define VALOR_ATE_1_HORA 30.0
#define VALOR_ATE_2_HORAS 45.0
#define VALOR_HORA_ADICIONAL 5.0

struct Carro {
    char placa[10];
    int hora_entrada;
    int minuto_entrada;
};

void registrarEntrada(struct Carro carros[], int *contador) {
    if (*contador == MAX_VAGAS) {
        printf("Estacionamento lotado!\n");
        return;
    }

    struct Carro novoCarro;

    printf("\n-------- Registrar entrada --------\n");
    printf("Digite a placa do veículo: ");
    scanf("%s", novoCarro.placa);

    printf("Digite o horário de entrada (hora minuto): ");
    scanf("%d %d", &novoCarro.hora_entrada, &novoCarro.minuto_entrada);

    carros[*contador] = novoCarro;
    (*contador)++;

    FILE *arquivo = fopen("carrosEstacionados.txt", "a");
    if (arquivo != NULL) {
        fprintf(arquivo, "%s %02d:%02d\n", novoCarro.placa, novoCarro.hora_entrada, novoCarro.minuto_entrada);
        fclose(arquivo);
    } else {
        printf("Erro ao abrir o arquivo!\n");
    }

    printf("Carro registrado com sucesso!\n");
}

void calcularTempo(int hora_entrada, int minuto_entrada, int hora_saida, int minuto_saida, int *horas, int *minutos) {
    int tempo_entrada = hora_entrada * 60 + minuto_entrada;
    int tempo_saida = hora_saida * 60 + minuto_saida;

    int diferenca = tempo_saida - tempo_entrada;

    *horas = diferenca / 60;
    *minutos = diferenca % 60;
}

double calcularValor(int horas, int minutos) {
    int tempo_total = horas * 60 + minutos;
    double valor = 0.0;

    if (tempo_total <= ISENTO) {
        valor = 0.0;
    } else if (tempo_total <= ATE_10_MIN) {
        valor = VALOR_ATE_10_MIN;
    } else if (tempo_total <= ATE_30_MIN) {
        valor = VALOR_ATE_30_MIN;
    } else if (tempo_total <= ATE_1_HORA) {
        valor = VALOR_ATE_1_HORA;
    } else if (tempo_total <= ATE_2_HORAS) {
        valor = VALOR_ATE_2_HORAS;
    } else {
        valor = VALOR_ATE_2_HORAS + ((horas - 2) * VALOR_HORA_ADICIONAL);
    }

    return valor;
}

void registrarSaida(struct Carro carros[], int *contador) {
    if (*contador == 0) {
        printf("Não há carros estacionados!\n");
        return;
    }

    char placa[10];
    int i, horas, minutos;
    double valor;
    int hora_saida, minuto_saida;

    printf("\n--------- Registrar saída ---------\n");
    printf("Digite a placa do veículo: ");
    scanf("%s", placa);

    printf("Digite o horário de saída (hora minuto): ");
    scanf("%d %d", &hora_saida, &minuto_saida);

    for (i = 0; i < *contador; i++) {
        if (strcmp(carros[i].placa, placa) == 0) {
            calcularTempo(carros[i].hora_entrada, carros[i].minuto_entrada, hora_saida, minuto_saida, &horas, &minutos);
            valor = calcularValor(horas, minutos);

            printf("\n----- Informações do veículo ------\n");
            printf("Placa: %s\n", carros[i].placa);
            printf("Tempo de uso: %dh %02dmin\n", horas, minutos);
            printf("Valor a pagar: R$ %.2f\n", valor);

            for (int j = i; j < *contador - 1; j++) {
                carros[j] = carros[j + 1];
            }

            (*contador)--;

            FILE *arquivo = fopen("carrosEstacionados.txt", "w");
            if (arquivo != NULL) {
                for (int j = 0; j < *contador; j++) {
                    fprintf(arquivo, "%s %02d:%02d\n", carros[j].placa, carros[j].hora_entrada, carros[j].minuto_entrada);
                }
                fclose(arquivo);
            } else {
                printf("Erro ao abrir o arquivo!\n");
            }

            return;
        }
    }

    printf("Carro não encontrado!\n");
}

void listarCarrosEstacionados(struct Carro carros[], int contador) {
    printf("\n------- Carros estacionados -------\n");
    if (contador == 0) {
        printf("Não há carros estacionados no momento.\n");
        return;
    }

    for (int i = 0; i < contador; i++) {
        printf("Placa: %s\n", carros[i].placa);
        printf("Hora de entrada: %02d:%02d\n", carros[i].hora_entrada, carros[i].minuto_entrada);
        printf("-----------------------------------\n");
    }
}

int main() {
    struct Carro carrosEstacionados[MAX_VAGAS];
    int contador = 0;
    int opcao;

    do {
        printf("\n----- Estacionamento Rotativo -----\n");
        printf("1. Registrar entrada de um carro\n");
        printf("2. Registrar saída de um carro\n");
        printf("3. Listar carros estacionados\n");
        printf("4. Sair\n");
        printf("-----------------------------------\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                registrarEntrada(carrosEstacionados, &contador);
                break;
            case 2:
                registrarSaida(carrosEstacionados, &contador);
                break;
            case 3:
                listarCarrosEstacionados(carrosEstacionados, contador);
                break;
            case 4:
                printf("Encerrando o programa...\n");
                break;
            default:
                printf("Opção inválida! Tente novamente.\n");
        }
    } while (opcao != 4);

    return 0;
}
