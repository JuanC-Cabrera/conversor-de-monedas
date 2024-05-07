
# Conversor de monedas

Challenge de la formación Java Orientado a Objetos G6 - del programa ONE


## API Reference (ExchangeRate-API )

Formato de el enlace: 
```http
  https://v6.exchangerate-api.com/v6/api_key/latest/moneda
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Your API key |



| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `moneda`      | `string` | **Required**. Clave de 3 siglas para el tipo de moneda |


Ejemplos de monedas "USD", "MXN", "BRL"


## Explicacion de las clases

### ConsultCurrency
En está clase es donde se lleva a cabo el consumo de la API y se guardan los valores de cambio en la clase Currency gracias a Gson().

```java
  return new Gson().fromJson(response.body(), Currency.class);
```

### Currency
En está clase se almacenan todos los tipos de cambio segun la moneda que se alla buscado en un HashMap de "conversion_rates" que se encuentra en la API.

```Java
 public class Currency {
    HashMap<String, Float> conversion_rates = new HashMap<>();

}
```

Se utiliza el HashMap ya que los datos se almacenan en forma de Llave-valor de la siguente foma: 

```json
"conversion_rates": {
    "MXN": 0.004352,
    "MYR": 0.001218,
    "MZN": 0.01639,
    ...
}
```
 ### Convert

 Es la clase que lleva a cabo la convercion de valores con la siguente funcion: 

```java
 public void convertCurrencies(String baseCode, String resultCode, double rate, Historial historial) {
        ConsultCurrency consult = new ConsultCurrency();
        Currency currency = consult.searchRate(baseCode);
        double finalResult = currency.conversion_rates.get(resultCode)*rate ;
        String mensaje = rate+" "+baseCode+" equivale a "+"%.2f"+" "+resultCode;
        System.out.printf((mensaje) + "%n", finalResult);
        historial.agregarAlHistorial(rate,baseCode,finalResult,resultCode);
    }
```
En ella se busca codigo de la base a la que se convertira en "conversion_rates" y se multiplica su valor por el valor que el usuario ingresa, para finalmente regresarel resultado en un printf y llamar a la funcion "agregarAlHistorial" de la clase Historial para guardar la consulta.

### Historial
Es la clase que almacena las consultas que se han realizado y las guarda en una ArrayList.

```java
  private List<String> historial = new ArrayList<>();
```

### Principal
Finalmente la clase principal es donde interactua el usuarion un menu para llevar a acabo las conversiones que se deseen.

```java
   String menu = """
                *******************************************
                Sea bienvenido/a al Conversor de Moneda :)
                1) Dólar --> Peso mexicano
                2) Peso mexicano --> Dólar
                3) Dólar --> Real brasileño
                4) Real brasileño --> Dólar
                5) Dólar --> Peso colombiano
                6) Peso colombiano --> Dólar
                7) Ver historial de conversiones
                8) Salir
                *******************************************
                Elija una opción:""";
```
## Author

- [@JuanC-Cabrera](https://github.com/JuanC-Cabrera)

