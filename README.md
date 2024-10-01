# testeAPI_Postman
Testando API com Postaman, com base no curso **Postman (2024): Do Zero ao Avançado + Testes Automatizados** da Udemy do Leonardo Adonis, SquadHub Academy. 

# Testando a API do OpenWeather no Postman, explicando cenário.

Feature: Consultar dados de clima através da API do OpenWeather
  Como um usuário da API do OpenWeather
  Eu quero consultar informações sobre o clima de uma cidade
  Para obter dados sobre temperatura, umidade, e descrição do clima

  Background:
    Dado que eu tenha a URL base da API do OpenWeather "https://api.openweathermap.org/data/2.5/weather"
    E que eu tenha uma chave de API válida
    E que eu tenha configurado um ambiente de testes no Postman
    
#Cenário com cidade válida: 
  Scenario: Consultar clima atual de uma cidade válida
    Dado que o nome da cidade seja "London"
    E que a unidade de medida seja "metric"
    Quando eu enviar uma requisição GET para a API do OpenWeather
    Então a resposta deve ter o status "200 OK"
    E o campo "name" na resposta deve ser "London"
    E o campo "main.temp" deve exibir a temperatura atual em Celsius
    E o campo "weather[0].description" deve conter a descrição do clima atual

  Scenario: Consultar clima de uma cidade inválida
    Dado que o nome da cidade seja "InvalidCity"
    Quando eu enviar uma requisição GET para a API do OpenWeather
    Então a resposta deve ter o status "404 Not Found"
    E o campo "message" deve ser "city not found"

  Scenario: Consultar clima sem chave de API
    Dado que o nome da cidade seja "London"
    E que eu não informe a chave de API
    Quando eu enviar uma requisição GET para a API do OpenWeather
    Então a resposta deve ter o status "401 Unauthorized"
    E o campo "message" deve ser "Invalid API key. Please see http://openweathermap.org/faq#error401 for more info."

#Cenário com cidade inválida: 
  Scenario: Consultar clima com chave de API inválida
    Dado que o nome da cidade seja "London"
    E que eu informe uma chave de API inválida
    Quando eu enviar uma requisição GET para a API do OpenWeather
    Então a resposta deve ter o status "401 Unauthorized"
    E o campo "message" deve ser "Invalid API key. Please see http://openweathermap.org/faq#error401 for more info."


## 1. Conta no Postaman

1. **Abrir o Postman**.
2. No menu lateral esquerdo, clique em **Collections**.
3. Clique em **New Collection** e nomeie sua coleção, por exemplo: `OpenWeather API Tests`.
4. Opcionalmente, adicione uma descrição para a collection para facilitar o entendimento sobre o que ela faz.

## 2. Obter a API Key do OpenWeather

1. Acesse o (https://openweathermap.org/api) e crie uma conta se ainda não tiver.
2. Após criar a conta e fazer login, vá até a seção de **API Keys** no seu perfil.
3. Gere uma nova chave de API e copie-a para utilizarmos nos testes.

## 3. Criar a primeira requisição: **Current Weather Data**

1. Na sua Collection, clique em **Add Request** e nomeie a requisição como `Get Current Weather`.
2. Selecione o método HTTP como **GET**.
3. No campo de URL, insira a seguinte URL da API do OpenWeather:
    ```
    https://api.openweathermap.org/data/2.5/weather
    ```
4. No canto direito da URL, clique no botão **Params**.
5. Adicione os seguintes parâmetros:
    - `q`: Nome da cidade que deseja buscar (Ex: `London`).
    - `appid`: Sua API Key obtida no passo 2.
    - `units`: Defina como `metric` para receber os dados no formato Celsius (opcional).
6. Exemplo de URL completa:
    ```
    https://api.openweathermap.org/data/2.5/weather?q=London&appid=SUA_API_KEY&units=metric
    ```

## 4. Configurar as Variáveis no Postman (Opcional)

Para facilitar a manutenção, você pode usar variáveis de ambiente no Postman para armazenar sua chave de API e o nome da cidade.

1. No canto superior direito do Postman, clique no ícone de olho (**Environment Quick Look**).
2. Clique em **Add** para criar um novo ambiente.
3. Adicione duas variáveis:
    - `city`: Defina o valor como o nome da cidade que deseja buscar (Ex: `London`).
    - `apikey`: Defina o valor como sua chave de API do OpenWeather.
4. No campo de URL da sua requisição, substitua os valores por variáveis:
    ```
    https://api.openweathermap.org/data/2.5/weather?q={{city}}&appid={{apikey}}&units=metric
    ```

## 5. Executar a Requisição

1. Com tudo configurado, clique em **Send** para enviar a requisição.
2. O Postman exibirá a resposta da API no painel inferior. A resposta deve conter informações sobre o clima atual da cidade requisitada, como temperatura, umidade, descrição do clima, etc.

Exemplo de resposta:

```json
{
  "coord": {
    "lon": -0.1257,
    "lat": 51.5085
  },
  "weather": [
    {
      "id": 300,
      "main": "Drizzle",
      "description": "light intensity drizzle",
      "icon": "09d"
    }
  ],
  "main": {
    "temp": 280.32,
    "feels_like": 278.4,
    "temp_min": 279.15,
    "temp_max": 281.15,
    "pressure": 1012,
    "humidity": 81
  },
  "name": "London",
  "cod": 200
}
