# Modelagem de API's seguras - OWASP

### API 

    Application Programming Interface(API) , serve como ‘interface’ padronizada  para comunicação  entre  sistemas ou aplicações para a partilha de dados encapsulando as tecnologias utilizadas.

    Contrariamente a uma ‘interface’ de utilizador que conecta um computador ao utilizador, uma ‘interface’ de programação conecta o computador ou pedaços de software  mutualmente.

    Constitui objectivo de uma API, abstrair detalhes de implementação interna, expondo somente partes, objectos ou ainda acções que se considera serem importantes para a comunicação. 

    

    ### Modelagem

    

    O modelo arquitectural permite o estabelecimento de um padrão para proporcionar uma melhor experiência para o desenvolvedor no consumo e integração.

    

    **Arquitectura REST & RESTFUL**

    A aplicação de um padrão fornece uma solução reutilizável para problemas comuns. A ideia e acelerar o processo de desenvolvimento através de um paradigma de desenvolvimento bem testado e adoptado.

    Portanto, o padrão REST visa padronizar as API's, tornando a  utilização e resposta dos recursos homogéneo, economizando esforço e aprendizagem dos desenvolvedores que consomem a API.

    Os princípios do REST envolvem resources e cada um deve possuir uma URI única. Esses resources são manipulados utilizando solicitações HTTP. E cada método (GET, POST, PUT, PATCH, DELETE) possui um significado específico.

    

    | Método Http | Descrição                                                    |      |
    | ----------- | ------------------------------------------------------------ | ---- |
    | **OPTIONS** | Retorna os verbos http de um resource e outras opções, como CORS, por exemplo. |      |
    | **GET**     | Busca um resource                                            |      |
    | **PUT**     | Actualiza um resource                                        |      |
    | **POST**    | Cria um resource                                             |      |
    | **DELETE**  | Remove um resource                                           |      |
    | **PATCH**   | Actualiza parcialmente um resource                           |      |
    | **HEAD**    | Busca apenas o header de um resource                         |      |

    

    ** Vocabulo **

    ```
    O método PUT é idempotente. Um método é considerado idempotente se o resultado de uma requisição realizada com sucesso é independente do número de vezes que é executada. Gregor Roth
    ```

    

    ** GET ** 

    O GET jamais deveria criar, actualizar ou apagar um recurso. O resultado do GET sempre será o mesmo para um determinado conjunto de informações.

    

    **PUT** 

    O PUT pode actualizar um recurso, mas o retorno do Servidor deve ser sempre igual. Independente de quantas vezes for feito a mesma chamada. Por exemplo, considere uma classe que possui o atributo nome:

    ```
    {
    "nome":"Maria"
    }
    ```

    Ao chamar o PUT solicitando que o nome seja alterado para João, na primeira vez o nome é alterado. Retorna 200 Ok do servidor. Nesse caso o recurso foi actualizado. Logo não é uma chamada Safe. No entanto, a partir da segunda chamada em diante, toda vez feito a solicitação para o nome ser João, o recurso não é alterado, porém, seu retorno deve ser sempre 200 Ok.

    

    **DELETE**

    O DELETE segue o mesmo princípio do PUT.

    

    **POST**

    Já o **POST** é o verbo mais sensível. Toda vez que é chamado um recurso pode ser criado. E se chamar 1000x o endpoint vai resultar em mil novos recursos similares. Por isso ele não é Safe. Ele também não é idempotente, pois o **POST** pode retornar parâmetros diferentes na resposta. Por exemplo, o parâmetro **Location** no Header da resposta. Esse parâmetro contém a URI onde pode ser localizado o recurso criado.

    **PATCH** Se o patch é similar ao PUT, por que ele é idempotente? É uma questão de definição. O PATCH não é Idempotente, enquanto o PUT é. Nesse caso o patch pode ser utilizado em uma chamada de actualização que tem resultados diferentes.

    Por exemplo:

    ```
     [ 
         {"operation": "replace", "field": "email", "value": "bruno@gmail.com"} 
     ]
    ```

    Na primeira chamada há uma integração com a Fábrica de crachá e retorna um 200 Ok. Na segunda chamada, há uma limitação que não pode alterar um recurso duas vezes em menos de 24hr. Tendo como resultado 400 Bad Request.

    

    ##### Endpoints

    Padrão de nomenclatura - Algumas dicas para padrão de nomenclatura.

    - Dê preferência para o plural ao disponibilizar o resource. Utilize `/users` ao invés de `/user`.
    - Se um recurso possui um nome composto utilize o kebab-case. Por exemplo Global Configuration seria `GET /global-configuration`.

    - Dê preferência para URL's em minúsculo, evite `GET /Users`, use `GET /users`.

    

    A raiz do resource deve retornar uma colecção. Por exemplo, `/users` deve retornar uma lista de usuários. Se desejar obter um **resource** especifico utilize o nível seguinte especificando seu identificador único. GET `/users/2`. Não precisa ser o id do banco, poderia ser outro campo, desde que seja identificador único. Um usuário poderia ser o username.

    - `GET /users` -> Retorna uma lista de usuários

    - `GET /users/bruno` -> Retorna o usuário com username bruno

    - `POST /users` -> Cria um usuário

    - `PUT /users/bruno` -> Actualiza o usuário Bruno

    - `PATCH /users/bruno` -> Actualiza parcialmente o usuário Bruno

    - `DELETE /users/bruno` -> Remove o usuário Bruno

      

    **Relacionamento filho** - Se existir alguma tabela filho do **resource**, eles devem estar mapeados para o mesmo endpoint. Exemplo:

    - `GET /users/bruno/claims` -> Retorna uma lista de claims do usuário bruno
    - `GET /users/bruno/claims/6` -> Retorna o claims com Id 6
    - `POST /users/bruno/claims` -> Cria uma claim para usuário Bruno
    - `PUT /users/bruno/claims/6` -> Actualiza a claim 6 do usuário Bruno
    - `PATCH /users/bruno/claims/6` -> Actualiza parcialmente a claim 6 do usuário Bruno
    - `DELETE /users/bruno/claims/6` -> Remove a claim 6 do usuário Bruno

    

    Há situações de relacionamento que não seguem essa hierarquia. Por exemplo, **departamento**. Um usuário pertence a um departamento, mas no design pode fazer sentido tanto **departamento** quanto **usuário** terem URI diferentes.

    

    OpenAPI

    

    O Open Api, anteriormente conhecido como Swagger, é hoje o padrão mais utilizado para documentar API's

    

    ### Segurança

    

    Segundo a  " Open Web  Application Security Project " - OWASP, uma comunidade aberta  sem fins lucrativos  que produz artigos, metodologias,  documentação, ferramentas,  e tecnologias para web. A OWASP disponibiliza   estratégias e soluções para entender e mitigar riscos de segurança em relação a api's nos 5 principais pilares fundamentais:

    - Visibilidade
    - Controle de acesso
    - Controle de tráfego
    - Protecção de Ameaças
    - Integridade & Confidencialidade

    

    Algumas práticas são recomendadas para garantir a segurança de APIs:

    

    - **Analisar as vulnerabilidades da API**

    Para garantir a **segurança de APIs**, é necessário que a verificação automática seja habilitada, a fim de detectar vulnerabilidades e eliminá-las nas diferentes fases do ciclo de vida do software. 

    Os recursos de verificação automatica permitem identificar falhas de segurança, na medida em que comparam a configuração do aplicativo com um banco de dados de vulnerabilidades conhecidas e divulgadas pelo OWASP.

    

    - **Restringir métodos HTTP**

    APIs REST habilitam programas capazes de executar diversas operações de HTTP. Informações de HTTP não são criptografados, sendo assim, esses métodos podem facilitar ataques. 

    Para ter mais segurança, é importante proibir métodos HTTP inseguros, mas caso  não seja possível, recomenda-se que  restrinja a lista de permissões, rejeitando todas as solicitações que não façam parte da lista. 

    Outra importante medida é o uso das práticas de autenticação da API RESTful, que garantem que o utilizador possa utilizar o método HTTP.

    

    - **Evite entradas não íntegras, implementando mecanismos de validação de entrada**

      recomendado que os profissionais de segurança da informação implementem mecanismos que permitam validar a entrada no servidor e no cliente de modo a evitar entradas não íntegras. 

    No que diz respeito ao cliente, essa validação tem a função de indicar erros e avisar sobre entradas que devem ser aceitas. No lado do servidor, serve para verificar os dados recebidos e evitar ameaças, como ataques *SQL Injection* e *XSS*. 

    

    - **Defina um limite máximo de solicitações** 

    A limitação de solicitações é uma medida de **segurança de APIs** que exige configurar um estado temporário para que a API analise as solicitações. Normalmente é utilizada para evitar abusos, spam ou ataques de negação de serviço. Ela também contribui para garantir a segurança da API REST  evitando ataques de força bruta e DDoS. 

    Algumas APIs podem ter limites flexíveis, possibilitando que os utilizadores ultrapassem os limites de requisições por um tempo curto. Por isso, definir o limite desse tempo é uma prática recomendada para garantir a **segurança de APIs**. 

    Além disso, as bibliotecas de filas de requisicoes possibilitam criar APIs que aceitam um número pré-definido de solicitações, colocando as demais em uma fila de espera. 

    

    -  **HTTPS/TLS devem ser utilizadas para APIs REST**

    HTTPS e *Transport Layer Security* (TLS) proporcionam segurança para a transferência de informações criptografadas entre servidores da web e navegadores. Além disso, o HTTPS contribui para a protecção de credenciais de autenticação em trafego. 

    É recomendado que toda API implemente HTTPS para garantir a confidencialidade, autenticidade e integridade. E mais, aconselha-se ainda que os profissionais de segurança utilizem certificados mutuamente autenticados do lado do cliente para proporcionar mais segurança para operações e informações confidenciais. 

    

    - **Utilizar  plataformas de gestão de API**

    Um gateway de API tem a finalidade de separar a interface do cliente da colecção de APIs de back-end, e garantir a disponibilidade e escalabilidade dos serviços de API. 

    Além de administrar os mais diversos serviços de API, a plataforma de gestão de API possibilita gerir funções padrão, como limitação de taxa, telemetria e autenticação de utilizador.

     O gateway de API é caracterizado por aceitar chamadas de API, coordenar recursos necessários para atendê-la, efectuar a autenticação, e garantir os resultados apropriados.

    

​      

  ### Fontes

- https://www.altexsoft.com/blog/engineering/what-is-api-definition-types-specifications-documentation/

- https://brightsec.com/blog/api-security/#:~:text=API%20security%20is%20the%20process,of%20modern%20web%20application%20security.

- https://en.wikipedia.org/wiki/API

- https://livebook.manning.com/book/api-security-in-action/chapter-1/22

- https://www.veracode.com/security/owasp-top-10

- https://senhasegura.com/pt-br/os-desafios-para-garantir-a-seguranca-de-apis/

- https://swagger.io/solutions/api-monitoring/

- https://www.brunobrito.net.br/api-restful-boas-praticas/
