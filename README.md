% =========================================================================
% RETO 2: MODELADO DEL BALANCE HÍBRIDO - SIGE
% Curso: Software para Ingeniería (Código 203036)
% Descripción: Simulación del balance energético de una micro-red (24 horas)
% =========================================================================

% Esta instrucción limpia las variables que estaban guardadas, limpia la
% ventana de comandos y cierra las figuras que estuvieran abiertas.
clear; clc; close all;

%% 1. Definición de la variable temporal
% Se crea un vector con las 24 horas del día, desde la hora 1 hasta la 24.
% Sirve como eje de tiempo para comparar generación y consumo.
tiempo = 1:24;

%% 2. Generación Solar (kW)
% Se crea un vector de 24 posiciones lleno de ceros para guardar la
% potencia solar de cada hora del día.
p_solar = zeros(1, 24);

% Horas de sol: 6 a 18
% Se define el grupo de horas en las que se supone que existe radiación
% solar. Son las horas desde la 6 hasta la 18.
horas_sol = 6:18;

% Se calcula una curva parecida a una onda para representar la generación
% solar durante las horas de sol. El valor máximo usado es de 25 kW.
% numel cuenta cuantos elementos tiene el vector de horas.
p_solar(horas_sol) = 25 * sin(pi * (0:numel(horas_sol)-1) / (numel(horas_sol)-1));

%% 3. Generación Eólica (kW)
% Este vector contiene los valores de potencia producida por el viento
% para cada una de las 24 horas. Los datos son puestos manualmente.
p_eolica = [8.5, 9.0, 10.2, 11.5, 9.8, 7.2, 5.0, 4.5, 6.1, 8.0, ...
    10.5, 12.0, 14.2, 13.5, 11.0, 9.5, 8.0, 10.1, 12.5, 13.0, ...
    11.8, 9.2, 8.0, 7.5];

%% 4. Demanda de la Comunidad (kW)
% Este vector guarda el consumo de energía de la comunidad para las
% diferentes horas del día. También tiene 24 valores.
p_demanda = [3.0, 2.5, 2.0, 2.5, 3.5, ...
    6.5, 7.0, 8.0, 8.5, 9.0, 9.5, 10.0, ...
    9.5, 8.5, 8.0, 7.5, 8.0, ...
    15.0, 15.0, 14.5, 14.0, ...
    4.0, 3.5, 3.0];

%% 5. Validación de dimensiones
% Se verifica que los tres vectores principales tengan exactamente 24
% datos. Si alguno no tiene 24, MATLAB muestra el mensaje de error.
% assert sirve para comprobar que una condición sea verdadera.
assert(numel(p_solar) == 24 && numel(p_eolica) == 24 && numel(p_demanda) == 24, ...
    'Los vectores deben tener 24 elementos.');

%% 6. Generación Híbrida Total (kW)
% Se suman la generación solar y la generación eólica hora por hora.
% El resultado representa toda la potencia generada por el sistema híbrido.
p_generacion_total = p_solar + p_eolica;

%% 7. Visualización Gráfica
% Se abre una nueva ventana para mostrar el gráfico del balance energético.
% También se coloca un nombre a la ventana y se quita el número automático.
figure('Name', 'Balance Energético SIGE', 'NumberTitle', 'off');

% Se dibuja la generación total usando el tiempo como eje horizontal.
% La b es azul, o pone circulos y LineWidth hace la línea más gruesa.
plot(tiempo, p_generacion_total, 'b-o', 'LineWidth', 2, 'MarkerFaceColor', 'b');

% hold on permite colocar otra gráfica encima de la primera sin borrarla.
hold on;

% Se dibuja la demanda de la comunidad en color rojo y con línea
% discontinua para poder distinguirla de la generación.
plot(tiempo, p_demanda, 'r--s', 'LineWidth', 2, 'MarkerFaceColor', 'r');

% Se termina la superposición de las gráficas.
hold off;

% Se coloca el título principal del gráfico y se configura su tamaño.
title('Balance Energético Diario - Sistema Inteligente de Gestión Energética (SIGE)', ...
    'FontSize', 12);

% Se coloca el nombre del eje horizontal, que representa las horas.
xlabel('Tiempo (Horas)', 'FontSize', 11);

% Se coloca el nombre del eje vertical, donde aparece la potencia en kW.
ylabel('Potencia (kW)', 'FontSize', 11);

% Se agrega una leyenda para saber cual línea representa generación y cual
% representa la demanda. La leyenda queda ubicada arriba a la izquierda.
legend('Generación Híbrida Total', 'Demanda Comunitaria', 'Location', 'northwest');

% Se activa una cuadricula para que sea más facil leer los valores del gráfico.
grid on;

% Se limita el eje X para mostrar solamente las horas de 1 a 24.
xlim([1 24]);

% Se indican todas las horas de 1 a 24 como marcas del eje X.
xticks(1:24);
