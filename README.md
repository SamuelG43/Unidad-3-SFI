# Unidad-3-SFI

## Ejercicio 1
- ¿Cómo se ve un protocolo binario?

   Un protocolo binario es un conjunto de reglas que define cómo se intercambia información entre dos o más fuentes utilizando representación en formato binario. A diferencia de los protocolos basados en texto, que utilizan caracteres legibles por humanos (como ASCII), los protocolos binarios están diseñados para ser leídos por máquinas, no por personas1. Estos protocolos utilizan todos los valores de un byte, lo que permite una mayor eficiencia en la transmisión de dato
  
- ¿Puedes describir las partes de un mensaje? ¿Para qué sirve cada parte del mensaje?

   _Emisor_: Es la persona o entidad que envía el mensaje. En un correo electrónico, esta parte corresponde al campo “De:” y contiene la dirección de correo electrónico del remitente.


   _Destinatario_: Representa a la persona o entidad a la que se dirige el mensaje. En un correo electrónico, se especifica en el campo "Para:".


   _Contenido_: Es el mensaje en sí, que puede incluir texto, imágenes, archivos adjuntos, etc. En la comunicación general, esta parte es fundamental para transmitir la información deseada.


   _Contexto_: Se refiere al contexto en el que se envía el mensaje. Comprender el contexto ayuda al destinatario a interpretar correctamente el contenido.


   _Canal_: Es el medio a través del cual se transmite el mensaje. Puede ser un correo electrónico, una llamada telefónica, una conversación cara a cara, etc..




  ## Ejercicio 2

- Serial.available():
 Esto es que revisa si hay alyun dato almacenado en el micro
- Serial.read():
Esto sirve para leer estos mismos datos que están almacenados 
- Serial.readBytes(buffer, length):
Esto es para leer y verificar los datos de los bytes como su tamaño 
- Serial.write():
Esto es para que imprima lo que está almacenado


## Ejercicio 3

- ¿Qué es un *Endian*?
  Un endian es la forma en la que se organizan los bytes dependiendo de su peso y existen dos (2) formas de organizar estos datos, en *little endian* o en *big endian* y tambien con una linea de código puede modificar esos valores para hacerlo de un modo u otro dependiendo de lo que se quiere lograr.


  ## Ejercicio 4

````
  void setup() {
    Serial.begin(115200);
}
void loop() {
    // 45 60 55 d5
    // https://www.h-schmidt.net/FloatConverter/IEEE754.html
    static float num = 3589.3645;

    if(Serial.available()){
        if(Serial.read() == 's'){
            Serial.write ( (uint8_t *) &num,4);
        }
    }
}

````
Serial.begin : Es la velocidad a la que se ejecutará el código

Los numeros 45 60 55 d5 don los valores hexadecimales del valor que se declara en static float = 3589.3645

Serial.write ( (uint8_t *) &num,4); convierte el valor de ese decimal en valores enteros 

````
void setup() {
    Serial.begin(115200);
}

void loop() {
// 45 60 55 d5// https://www.h-schmidt.net/FloatConverter/IEEE754.htmlstatic float num = 3589.3645;
static uint8_t arr[4] = {0};

if(Serial.available()){
if(Serial.read() == 's'){
            memcpy(arr,(uint8_t *)&num,4);
            Serial.write(arr,4);
        }
    }
}
````

Acá se declara un arreglo "static uint8_t arr[4] = {0};" de 4 posiciones empezando por la posición cero (0) pasando por uno, dos y tres

memcpy(arr,(uint8_t *)&num,4);  así como en la parte anterior esto sirve para cambiar el valor de decimales a enteros 

 Serial.write(arr,4); Este sirve para imprimir ese arreglo de las 4 posiciones previamente establecidas


- ¿En qué endian estamos transmitiendo el número?
              En este caso estamos transmitiendo en un *big endian*

  
 - Y si queremos transmitir en el endian contrario, ¿Cómo se modifica el código?
            Se le agregaría un ciclo "for" como se muestra a continuación "for(int8_t i = 3; i >= 0; i--){
              Serial.write(arr[i]);"

````
void setup() {
    Serial.begin(115200);
}

void loop() {
// 45 60 55 d5// https://www.h-schmidt.net/FloatConverter/IEEE754.htmlstatic float num = 3589.3645;
static uint8_t arr[4] = {0};

if(Serial.available()){
if(Serial.read() == 's'){
            memcpy(arr,(uint8_t *)&num,4);
for(int8_t i = 3; i >= 0; i--){
              Serial.write(arr[i]);
            }
        }
    }
}
````

## Ejercicio 6

```
SerialPort _serialPort =new SerialPort();
_serialPort.PortName = "/dev/ttyUSB0";
_serialPort.BaudRate = 115200;
_serialPort.DtrEnable =true;
_serialPort.Open();
```

```
byte[] data = { 0x01, 0x3F, 0x45};
_serialPort.Write(data,0,1);
```

```
byte[] buffer =new byte[4];
.
.
.

if(_serialPort.BytesToRead >= 4){

    _serialPort.Read(buffer,0,4);
for(int i = 0;i < 4;i++){
        Console.Write(buffer[i].ToString("X2") + " ");
    }
}
```

## Trabajo Final Unidad 3

La documentación de este ejercicio está en los archivos del repositorio como pdf.

- Código Arduino
```
unsigned long previousMillis = 0;
const long interval = 1000; // Intervalo de 1 segundo (en milisegundos)

   float variable1 = 20.0;
   float variable2 = 10.0;
   float deltaChangue = 2.0;


void setup()
{
  Serial.begin(115200);
    adc_init();
  adc_set_temp_sensor_enabled(true);
  
}

void loop()
{


   uint8_t arr[4] = {0};
   static bool variable1Habilitada = true;
   static bool variable2Habilitada = true;
   static bool temperaturaHabilitada = true;
   float variable1HabilitadaNumero;
   float variable2HabilitadaNumero;
  float temperaturaHabilitadaNumero;
   float temp = 0.0;

     adc_select_input(4);
      uint16_t result = adc_read();
        float voltage = result * (3.3f /4096);
        if (temperaturaHabilitada == true)
        {
         temp = 27 - (voltage - 0.706) / 0.001721;
        }
        else
        {
          
        }


   //-------------------     
    if(variable1Habilitada == true)
    {
      variable1HabilitadaNumero = 1.0;
    }
    else
    {
       variable1HabilitadaNumero = 0.0;

    }
    //--------------
    
     if(variable2Habilitada == true)
    {
      variable2HabilitadaNumero = 1.0;
    }
    else
    {
       variable2HabilitadaNumero = 0.0;

    }
    //--------------------
         if(temperaturaHabilitada == true)
    {
      temperaturaHabilitadaNumero = 1.0;
    }
    else
    {
       temperaturaHabilitadaNumero = 0.0;

    }
        
 unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
        previousMillis = currentMillis;
        if (variable1Habilitada == true)
        {
         variable1 -= 2.0;
        }

            if (variable2Habilitada == true)
        {
         variable2 -= 2.0;
        }
   
        
        
    }

    
    //logica para el envio de datos
    char TeclaRecibida;
        
        if (Serial.available() > 0)
        {
            TeclaRecibida = Serial.read();

           if (TeclaRecibida == '1')
            {
                memcpy(arr,(uint8_t *)&variable1,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                memcpy(arr,(uint8_t *)&variable2,4);
                for(int8_t i = 0; i < 4; i++)
                  Serial.write(arr[i]);


                memcpy(arr,(uint8_t *)&temp,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }
                
                memcpy(arr,(uint8_t *)&variable1HabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                       memcpy(arr,(uint8_t *)&variable2HabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }
                       memcpy(arr,(uint8_t *)&temperaturaHabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                                       memcpy(arr,(uint8_t *)&deltaChangue,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

 
  
          
            }

            if (TeclaRecibida == '2')
            {
              variable1Habilitada = true;
            }
            
            if (TeclaRecibida == '3')
            {
              variable2Habilitada = true;
            }
            
            if (TeclaRecibida == '4')
            {
              temperaturaHabilitada = true;                            
            }

            if (TeclaRecibida == '5')
            {
              variable1Habilitada = false;
            }
            
            if (TeclaRecibida == '6')
            {
              variable2Habilitada = false;
            }
            
            if (TeclaRecibida == '7')
            {
              temperaturaHabilitada = false;                            
            }
  
       }
}
```
- Código Unity

```
using UnityEngine;
using System.IO.Ports;
using TMPro;
using UnityEngine.UI;
using System;


public class UTPTPA : MonoBehaviour
{
    private SerialPort _serialPort = new SerialPort();

    public TextMeshProUGUI variable1Texto;
    public TextMeshProUGUI variable2Texto;
    public TextMeshProUGUI temperaturaTexto;

    public TextMeshProUGUI habilitadovariable1Texto;
    public TextMeshProUGUI habilitadovariable2Texto;
    public TextMeshProUGUI habilitadotemperaturaTexto;
    public TextMeshProUGUI deltaChangueTexto;

    public float habilitadovariable1;
    public float habilitadovariable2;
    public float habilitadotemperatura;
    public float deltaChangue;
    void Start()
    {
        _serialPort.PortName = "COM4";
        _serialPort.BaudRate = 115200;

        _serialPort.DtrEnable = true;
        _serialPort.Open();
        Debug.Log("Puerto serial listo");
    }

    
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.S))
        {
            byte[] data = { 0x31 };
            _serialPort.Write(data, 0, 1);
            byte[] buffer = new byte[28];
            Debug.Log("S");
            if (_serialPort.BytesToRead >= 4)
            {
                _serialPort.Read(buffer, 0, 28);

            variable1Texto.SetText("");
            variable2Texto.SetText("");
            temperaturaTexto.SetText("");
            
           float variable1 = BitConverter.ToSingle(buffer, 0);
            float variable2 = BitConverter.ToSingle(buffer, 4);
            float temperatura = BitConverter.ToSingle(buffer, 8);

            variable1Texto.text = variable1.ToString();
            variable2Texto.text = variable2.ToString();
            temperaturaTexto.text = temperatura.ToString();

            habilitadovariable1Texto.SetText("");
            habilitadovariable2Texto.SetText("");
            habilitadotemperaturaTexto.SetText("");
            deltaChangueTexto.SetText("");
            
             habilitadovariable1 = BitConverter.ToSingle(buffer, 12);
             habilitadovariable2 = BitConverter.ToSingle(buffer, 16);
             habilitadotemperatura = BitConverter.ToSingle(buffer, 20);
             deltaChangue = BitConverter.ToSingle(buffer, 24);

            habilitadovariable1Texto.text = habilitadovariable1.ToString();
            habilitadovariable2Texto.text = habilitadovariable2.ToString();
            habilitadotemperaturaTexto.text = habilitadotemperatura.ToString();
            deltaChangueTexto.text = deltaChangue.ToString();
            }
            
        }

        

        if (Input.GetKeyDown(KeyCode.Q))
        {
            byte[] data = { 0x32 };
            _serialPort.Write(data, 0, 1);
        }

        
        if (Input.GetKeyDown(KeyCode.W))
        {
            byte[] data = { 0x33 };
            _serialPort.Write(data, 0, 1);
        }

        
        if (Input.GetKeyDown(KeyCode.E))
        {
            byte[] data = { 0x34 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.R))
        {
            byte[] data = { 0x35 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.T))
        {
            byte[] data = { 0x36 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.Y))
        {
            byte[] data = { 0x37 };
            _serialPort.Write(data, 0, 1);
        }
    }
}
```
  
