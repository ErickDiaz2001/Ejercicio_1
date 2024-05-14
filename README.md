Introducción

Este informe describe el diseño e implementación de un sistema de control para una puerta corrediza automática accionada por presencia. El sistema utiliza un sensor de presencia para detectar la presencia de personas y dos leds que indican la apertura y cierra de la puerta y el estado de la puerta: cerrada o abierta. La puerta se abre cuando se detecta presencia y permanece abierta durante 3 segundos después de que la persona ha pasado. Si se detecta presencia mientras la puerta se está cerrando, la puerta se detiene y se abre nuevamente.

Configuracion 

El system clock del microcontrolador se configuro a 72Mhz.

Se utilizara el timer 2, para obtener una interrupción cada 1 segundo se configuro un prescaler a 7200-1 y el counter period a 10000. Ademas de habilitar la interrupcion. 

![image](https://github.com/ErickDiaz2001/Ejercicio_1/assets/169405943/00dc879d-16f7-42ea-97c6-c1b08e5214e5)

Metodología

Se implementaran maquinas de estados para la resolucion de este ejercicio.

![image](https://github.com/ErickDiaz2001/Ejercicio_1/assets/169405943/fbb3917e-033d-4277-9e26-9bc6a3dfb106)

El microcontrolador monitorea la salida del sensor y controla el encendido de LED_1 y LED_2 que indica si el motor esta abriendo o cerrando la puerta y LED_2 que indica el estado del motor apagado o encendido. 

El estado inicia es PUERTA CERRADA, cuando se detecta presencia SENSOR = 1 se pasa al estado ABRIENDO PUERTA, se activa LED_1 y LED_2 que indica que el motor está abriendo y esta encendido. Si el FINAL DE CARRERA ABIERTO es activado pasa al estado PUERTA ABIERTA donde el LED_2 se apaga indicando que el motor esta apagado y se activa el CONTADOR.

Si se detecta una presencia SENSOR = 1 y el CONTADOR es menor a los 3 segundo vuelve al estado PUERTA ABIERTA.
Cuando el contador sea mayor a los 3 segundos y no se detecte presencia pasa al estado CERRANDO PUERTA donde el LED_1 se apaga y el LED_2 se enciende indicando que el motor está cerrando la puerta y esta encendido, ademas se resetea el CONTADOR.
Cuando el FINAL DE CARRERA CERRADO se activa pasa al estado PUERTA CERRADA donde el LED_2 se apaga indicando que el motor está apagado.

Resultados

https://youtube.com/shorts/yCf_ukVzNoY?si=mznmGouvjFMN9USW

Primera parte del video

  case PUERTA_ABIERTA:
	 		  HAL_GPIO_WritePin(GPIOA, LED_1_Pin, GPIO_PIN_SET); // abriendo puerta
	 		  HAL_GPIO_WritePin(GPIOA, LED_2_Pin, GPIO_PIN_RESET); // motor apagado
	 		  // cuenta es mayor a 3 segundos y no se detecta presencia
	 		  if (tim_count > TIEMPO_SIN_PRESENCIA && HAL_GPIO_ReadPin(GPIOA, SENSOR_PRESENCIA_Pin) == 0)
	 		  {
	 			  tim_count = 0;
	 			  estadoActual = CERRANDO_PUERTA;
	 		  }
	 		 // cuenta es menor a 3 segundos y se detecta presencia
	 		  else if (tim_count <= TIEMPO_SIN_PRESENCIA && HAL_GPIO_ReadPin(GPIOA, SENSOR_PRESENCIA_Pin) == 1)
	 		  {
	 			  tim_count = 0;
	 			  estadoActual = PUERTA_ABIERTA;
	 		  }


Segunda parte del video 

  case PUERTA_ABIERTA:
	 		  HAL_GPIO_WritePin(GPIOA, LED_1_Pin, GPIO_PIN_SET); // abriendo puerta
	 		  HAL_GPIO_WritePin(GPIOA, LED_2_Pin, GPIO_PIN_RESET); // motor apagado
	 		  ...
	 		 // cuenta es menor a 3 segundos y se detecta presencia
	 		  else if (tim_count <= TIEMPO_SIN_PRESENCIA && HAL_GPIO_ReadPin(GPIOA, SENSOR_PRESENCIA_Pin) == 1)
	 		  {
	 			  tim_count = 0;
	 			  estadoActual = PUERTA_ABIERTA;
	 		  }

Conclusiones

La implementación de máquinas de estado para un sistema de control para una puerta corrediza automática con detección de presencia ofrece varias ventajas:

Mayor previsibilidad al estructurar un sistema

Reducion la complejidad del código separando la lógica en bloques discretos

Permite identificar de manera rápida y eficiente los errores.
