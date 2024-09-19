# BLACKJACK-C
Integrantes del equipo 
Angel Garcia Almeyda

Juan Carlos Escamilla Carballo

Oscar Manuel Martinez Ruiz

Jesús David Tejeda Hernández 

Lizbeth Guadalupe Segundo Cienega 

A simple Blackjack game in C++
#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
using namespace std;

// Función para obtener el valor de una carta
int obtenerValorCarta() {
    return rand() % 11 + 1;  // Cartas entre 1 y 11 (para simplificar)
}

// Función para mostrar las cartas del jugador o la computadora
void mostrarCartas(const vector<int>& cartas, bool esComputadora = false, bool mostrarTodas = true) {
    cout << (esComputadora ? "Cartas de la computadora: " : "Tus cartas: ");
    for (size_t i = 0; i < cartas.size(); ++i) {
        if (esComputadora && !mostrarTodas && i == 0) {
            cout << "[Oculta] ";
        } else {
            cout << cartas[i] << " ";
        }
    }
    cout << endl;
}

// Función para calcular el puntaje total de un jugador
int calcularPuntaje(const vector<int>& cartas) {
    int puntaje = 0;
    for (int carta : cartas) {
        puntaje += carta;
    }
    return puntaje;
}

// Función principal del juego
void jugarBlackJack() {
    srand(time(0));  // Semilla para generar cartas aleatorias
    
    vector<int> cartasJugador;
    vector<int> cartasComputadora;
    
    // Repartir dos cartas iniciales
    cartasJugador.push_back(obtenerValorCarta());
    cartasJugador.push_back(obtenerValorCarta());
    
    cartasComputadora.push_back(obtenerValorCarta());
    cartasComputadora.push_back(obtenerValorCarta());

    // Mostrar las cartas del jugador y la primera carta de la computadora
    mostrarCartas(cartasJugador);
    mostrarCartas(cartasComputadora, true, false);

    // Turno del jugador
    char opcion;
    while (true) {
        cout << "¿Deseas pedir otra carta? (s/n): ";
        cin >> opcion;
        
        if (opcion == 's' || opcion == 'S') {
            cartasJugador.push_back(obtenerValorCarta());
            mostrarCartas(cartasJugador);
            
            int puntajeJugador = calcularPuntaje(cartasJugador);
            cout << "Tu puntaje: " << puntajeJugador << endl;
            
            if (puntajeJugador > 21) {
                cout << "Has perdido, tu puntaje es mayor a 21." << endl;
                return;
            } else if (puntajeJugador == 21) {
                cout << "¡Has ganado con un BlackJack!" << endl;
                return;
            }
        } else {
            break;
        }
    }

    // Turno de la computadora
    while (calcularPuntaje(cartasComputadora) < 17) {  // La computadora pide cartas hasta tener 17 o más
        cartasComputadora.push_back(obtenerValorCarta());
    }

    mostrarCartas(cartasComputadora, true, true);
    int puntajeComputadora = calcularPuntaje(cartasComputadora);
    cout << "Puntaje de la computadora: " << puntajeComputadora << endl;

    // Determinar el resultado del juego
    int puntajeJugador = calcularPuntaje(cartasJugador);
    if (puntajeComputadora > 21) {
        cout << "¡Has ganado! La computadora ha superado 21." << endl;
    } else if (puntajeJugador > puntajeComputadora) {
        cout << "¡Has ganado! Tu puntaje es mayor." << endl;
    } else if (puntajeJugador < puntajeComputadora) {
        cout << "La computadora ha ganado." << endl;
    } else {
        cout << "Empate." << endl;
    }
}

int main() {
    char jugarDeNuevo;
    do {
        jugarBlackJack();
        cout << "¿Quieres jugar otra ronda? (s/n): ";
        cin >> jugarDeNuevo;
    } while (jugarDeNuevo == 's' || jugarDeNuevo == 'S');
    
    cout << "Gracias por jugar BlackJack!" << endl;
    return 0;
}

