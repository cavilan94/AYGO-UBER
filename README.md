En este documento se explicará el funcionamiento de la aplicación desplegada en AWS que emula una plataforma de servicio de alquiler viajes como UBER, DIDI u otros, como se hizó la aplicación y las pruebas realizadas.

Arquitectura de la aplicación:

![image](https://github.com/user-attachments/assets/4bcf4fba-4d9a-4418-be29-3bffba0a10df)

En la parte de front ent se tiene un mapa en HTML, el cual esta desplegado en AWS por medio del modulo AWS Amplify, en el mapa se realizara la simulación de un viaje entre un punto de inicio y un punto destino, las coordenadas para eld esplazamiento son calculadas en el back end por mdeio de una funcion lambda en AWS, la cual es consultada por medio de un API rest, usando un metodo GET, esas coordenadas son usadas en el mapa para generar los desplazamientos durante la simulación del viaje.
¿Como se hizó?

Se creo un mapa en HTML sobre el cual al dar click se marcara un punto de inicio y al dar un segundo click se marca un punto destino, el mapa utilizado es uno gratuito sumistrado por leflead
![image](https://github.com/user-attachments/assets/a6801575-793b-4c10-94b4-8ce16174802f)
Este mapa como punto de referencia esta centrado en Bogota Calle 127, este sera el punto donde inicializara el mapa cada vez que se acceda al servicio.
El mapa cuenta con boton llamado "iniciar viaje" el cual al darle click traza una linea entre los puntos de inicio y final y carga un icono en forma de carro que recorrera la linea trazada para simular el desplazamiento durante el viaje.

Las posiciones que asume el carro a medida que se va desplazando por la línea trazada son calculadas por medio de una función Lambda, la cual recibira las coordenadas de inicio y destino, calculara la pendiente entre esos puntos y devolvera valores de desplazamiento para recorrer la linea trazada, con aumentos de 0.1 grados en el sentido inicio a destino, la función creada se llama "viajes" y tiene la configuración que se muestra a continuación:

![image](https://github.com/user-attachments/assets/6dd425f9-6ee2-4386-84c2-28e143343934)

Se probó la función por medio de un test event

![image](https://github.com/user-attachments/assets/7deea77a-db07-46bf-a1e5-94a6212569dc)

El resultado del test event fue una ejecución exitosa con todas las coordenadas que debe tomar el carro para su movimiento a lo largo del recorrido.

![image](https://github.com/user-attachments/assets/f9bd06a8-ad34-4752-b7e1-23fbac258fbf)

Esta función Lambda sera llamada por medio de un API REST usando un metodo GET, la aplicación creada se llama "viajes" y tiene la siguiente configuración:

![image](https://github.com/user-attachments/assets/8f8ae46a-70a8-4bdd-af31-9687dbdc86a5)

Al momento de crear el recurso /ruta se seleccionó el recuadro CORS para evitar problemas de compilación, al hacer llamados o solicitudes desde un navegador.
![image](https://github.com/user-attachments/assets/23a634a7-c36e-4e86-814f-614d3647f9cf)

Al momento de crear el emtodo se escoge GET, se selecciona la aplicaicón lambda creada y se selecciona la opcion usar LAMBDA proxy, para que las solicitudes a la función lambda como un evento estructurado.

![image](https://github.com/user-attachments/assets/55d8b1fb-d774-4fde-820e-35ab94eb8a7a)

Luego se despliega la función y se asigna el nombre a la nueva etapa como prod
![image](https://github.com/user-attachments/assets/9ce0bc1e-4fbf-47af-a560-6635a31f6aaf)

Desde el lado del mapa en HTML usando un codigo en JS se hace un llamado a la función lambda por emdio del API rest creado, enviando las coordenadas para poder calcular los valores.

![image](https://github.com/user-attachments/assets/39c7f53b-b30e-469f-84be-d40e0401e9d2)

Las coordenadas son utilizadas por una función que generara el movimiento del carro

![image](https://github.com/user-attachments/assets/71c93782-2a40-457c-8900-6ffd2da8a763)

El mapa en HTML es desplegado usando AWS Amplify por medio de un repositorio en github

![image](https://github.com/user-attachments/assets/0cd1dc81-29eb-4a37-95c0-3ec3cfb57a60)

Al Acceder al mapa desplegado en AWS Amplify se muestra como se ve a continuación:

![image](https://github.com/user-attachments/assets/9749e0b0-e6d2-4dfa-8f44-98e2469684db)

Se marcan las coordenadas de inicio y destino, y al darle click en el boton "iniciar viaje", de traza la linea entre los puntos y se hace el llmado a la función lambda para calcular los valores de desplazamiento, con los valores generados se inicia la animación del desplazamiento del icono del carro.

![image](https://github.com/user-attachments/assets/a05ca3f8-eaf2-4ecf-a3be-e97d5421af6b)

![image](https://github.com/user-attachments/assets/d9a1b76c-a147-4c77-a33b-1426927eb57b)

![image](https://github.com/user-attachments/assets/a7517646-09ae-4628-9f53-3cb3761e6729)

![image](https://github.com/user-attachments/assets/df8f1718-8311-4864-bcb7-87a6b93e17d2)

Tambien se cuenta con un boton llamado "reiniciar" el cual refrezca la pantalla apra poder seleccionar nuevos puntos de inicio y destino.



